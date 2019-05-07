# ![LOGO](logo.png) Lumminary **flow**ground Connector

## Description

A generated **flow**ground connector for the Lumminary API (version 1.0).

Generated from: https://api.apis.guru/v2/specs/lumminary.com/1.0/swagger.json<br/>
Generated at: 2019-05-07T17:42:49+03:00

## API Description

# Introduction
The Lumminary API was built to allow third parties to interact with Lumminary customers and gain access to their genetic data. The Lumminary API is fast, scalable and highly secure. All requests to the Lumminary API take place over SSL, which means all communication of Customer data is encrypted.

Before we dive in, some definitions. This is what we mean by:

|Term|Definition|
|-----------|-----------|
|**Third party**|A third party (also referred to as "partner" or as "you") is a company which offers services and products using genetic data.|
|**Lumminary clients**|The Lumminary client (also referred to as "customer") is an individual who has created an account on the Lumminary platform.|
|**Lumminary**|This is  us - our services including the Lumminary platform, the API, the DNA App Store, the Vault, the "Connect with Lumminary" button, and the website in its totality. |
|**CWL**|This is the acronym for the "Connect with Lumminary" button.|
|**dataset**|This is the term we use when we refer to a customer's genetic data.|

Let's dive in, now. 
* [**Overview**](#section/Introduction/Overview)

* [**Install Lumminary API Client and Toolkit**](#Install-Lumminary-API-Client-andor-Toolkit)
* [**Obtaining credentials**](#Obtaining-credentials)
* [**Query customers authorizations**](#Query-customers-authorizations)
* [**Query customer genetic data**](#Query-customer-genetic-data)
* [**Submit reports**](#Submit-reports)

* [**"Connect with Lumminary" button**](#the-connect-with-lumminary-button)

* [**API specs**](#tag/Lumminary)

## Overview

In order to use Lumminary services, you'll need to install the Lumminary API Client and Toolkit. The Lumminary API Client and Toolkit are available in multiple programming languages, and we also provide a sandbox environment which the partner can use for integration and tests.

There are a couple of differences between the API Client and the Toolkit. Mainly, it's about the ease of use for integration. The Toolkit is basically a stand-alone application that facilitates the integration with the Lumminary API without the need to modify your already existing code.

You use the Lumminary API Client when you want to integrate it inside your own application. This means it gives you full flexibility regarding the integration into your own workflow.

You use the Lumminary Toolkit for an integration where the Toolkit is placed alongside your own application. You can use the Toolkit from the CLI - for example, to run a cronjob that processes incoming orders. The Toolkit has the Lumminary API Client built in, which means you don't have to separately install the API Client if you already have the Toolkit.

# Install Lumminary API Client and/or Toolkit

We provide the Lumminary API Client and Toolkit in multiple programming languages - default are PHP (minimum version 5.5), Python2.7 and Python3. However, if you need them in another language (Java, Obj-C, JavaScript, C#, Perl, CURL), please contact us.


## How to install the Lumminary API Client

#### PHP example:
The PHP Lumminary API Client is available at: https://github.com/Lumminary/lumminary-api-client-php

If you are already using [Composer](https://getcomposer.org), you can import the project by adding the following to your `composer.json`
```json
"repositories": [
    {
        "type": "git",
        "url": "https://github.com/Lumminary/lumminary-api-client-php.git",
        "reference": "master"
    }
],
"require": {
    "lumminary/api-client-php": "v1.0.0"
}
```

Then run `composer update lumminary/api-client-php`

#### Python example:

The Python Lumminary API Client is available at: https://github.com/Lumminary/lumminary-api-client-python

To install directly, run

```bash
pip install git+ssh://git@github.com/Lumminary/lumminary-api-client-python.git@v1.0.0#egg=lumminary-api-client
```

Or add the following line in your requirements.txt

```bash
git+ssh://git@github.com/Lumminary/lumminary-api-client-python.git@v1.0.0#egg=lumminary-api-client
```

## How to install the Lumminary Toolkit

#### PHP example:
The PHP Lumminary Toolkit is available at: https://github.com/Lumminary/lumminary-toolkit-php

To install the Lumminary Toolkit, run the following command where 'lumminary-toolkit-directory' is the directory where you want to install the Lumminary Toolkit:

`git clone git@github.com:Lumminary/lumminary-toolkit-php.git lumminary-toolkit-directory`

#### PYTHON example:

The Python Lumminary Toolkit is available at: https://github.com/Lumminary/lumminary-toolkit-python

To install the Lumminary Toolkit, run the following command where 'lumminary-toolkit-directory' is the directory where you want to install the Lumminary Toolkit:

`git clone git@github.com:Lumminary/lumminary-toolkit-python.git lumminary-toolkit-directory`

## Toolkit Setup

After cloning the Lumminary Toolkit project for your language, you need to configure it with your credentials. 

Go to the Lumminary Toolkit base diretory `cd lumminary-toolkit-directory`. Under the Toolkit directory, you will find a file `config/config_template.json` which has the following structure:

```json
{
    "api_key": <your-api-key>,
    "product_uuid": <your-product-uuid>,
    "api_host": "https://api.lumminary.com/v1",
    "output_root": <your-authorization-data-basepath>,
    "export_handler": <your-export-handler-class>,
    "optional": <your-optional-params>
}
```

You should copy this config `cp config/config_template.json config/my-lumminary-toolkit-config.json` then edit it `vim config/my-lumminary-toolkit-config.json` to contain the following values:

| Config attribute | Example Value                      | Description |
|------------------|------------------------------------|-------------|
| api_key        | `"TiiU...bqg=="`                   |Your Lumminary API key.<br> You can obtain it from the Lumminary Admin  |
| product_uuid   | `"1234-1234-1234-1234"`            |Your product UUID. This value can be obtained from the Lumminary Admin |
| api_host       | `"https://api.lumminary.com/v1"`   |   The API endpoint to use.<br>For the sandbox environment you can use "https://api.lumminary.com/v1"    |
| output_root    | `"/var/lumminary-orders/"`         | The base directory where data for Authorizations that your Product has received via Lumminary will be saved <br> The Lumminary Toolkit will *never* overwrite authorization data or create this directory, to protect from overwrites or typos    |
| export_handler |`"export_handler_tsv"`  | If you have a custom export handler, then your Lumminary contact will provide you with the name of your export handler.   |
| optional       | `{}`                               | Export handler specific value |

After updating the config file for your Toolkit, it should look similar to (Note that these are all dummy values) :

```json
{
    "api_key": "TiiU...bqg==",
    "product_uuid": "1234-1234-1234-1234",
    "api_host": "https://api.lumminary.com/v1",
    "output_root": "/var/lumminary-orders/",
    "export_handler": "export_handler_tsv",
    "optional": {}
}
```

You can now save and exit the text editor `:wq` and start polling the API for new Authorizations :

Python

```bash
# While still in the <lumminary-toolkit> directory
python lumminary_api_toolkit.py --config-path config/my-lumminary-toolkit-config.json
```

PHP

```bash
# While still in the <lumminary-toolkit> directory
php lumminary_api_toolkit.py --config-path config/my-lumminary-toolkit-config.json
```

When your Product receives new Authorizations, the Toolkit will pull all the relevant data and save it in the following files:

```bash
# The DNA data file. Format compatible with 23AndMe by default
<output_root>/<authorization_uuid>/dna-data.tsv

# The authorization metadata
<output_root>/<authorization_uuid>/authorization.json
```

The contents of the files pulled when processing an Authorization are as follows:

```bash
$ head -n 5 <output_root>/<authorization_uuid>/dna-data.tsv
# rsid  chromosome      position        genotype
rs12070387      1       6267531 CC
rs149124387     1       12025561        CC
rs116458387     1       14920119        AA
rs4436387       1       15498452        CC

$ cat <output_root>/<authorization_uuid>/authorization.json 
{
    "authorization": <authorization_uuid>,
    "created_timestamp_utc": 1542920184,
    "customer": "1234-1234-1234-1234",
    "customer_address": {
        "address1": "69 Vine St.",
        "address2": null,
        "city": "Parsippany",
        "country": "US",
        "state": "NJ",
        "zipcode": "07054"
    },
    "customer_email": "3712014126519136@incognito.lumminary.com",
    "customer_name": {
        "first_name": <customer_first_name>,
        "last_name": <customer_last_name>
    },
    "dataset": "2345-2345-2345-2345",
    "product": <your_product_uuid>
}
```

*Note:* We recommend running the Toolkit in a cronjob, wrapped by some locking mechanism. Also, Toolkit logs are very minimal but can be very helpful when debugging an issue, so please consider saving them to a file. For example

```bash
# Open the crontab
crontab -e
```

Python Toolkit

```bash
# Add the following line (replace <lumminary-toolkit-directory> with where you cloned the Lumminary Toolkit)
* * * * * flock /var/lock/lumminary-toolkit.lock python <lumminary-toolkit-directory>/lumminary_api_toolkit.py --config-path <lumminary-toolkit-directory>/config/my-lumminary-toolkit-config.json
```

PHP Toolkit

```bash
# Add the following line (replace <lumminary-toolkit-directory> with where you cloned the Lumminary Toolkit)
* * * * * flock /var/lock/lumminary-toolkit.lock php <lumminary-toolkit-directory>/lumminary_api_toolkit.php --config-path <lumminary-toolkit-directory>/config/my-lumminary-toolkit-config.json >> /var/log/lumminary-toolkit.log
```

## API endpoints
Lumminary provides two endpoint APIs, sandbox and production. You can use the sandbox for your integration and testing, simulate orders, upload genetic data, and generate reports. The sandbox works exactly like the production environment, and you can test your end-to-end integration.

In order to simulate a complete order, you need to use this test credit card: 

|Credit card number  | Expiration date| CVV2|
|--------------------|----------------|-----|
|4242 4242 4242 4242 |  12/30         | 123 |

### Sandbox
website: [sandbox-www.lumminary.com](https://sandbox-www.lumminary.com)

api-hostname: [sandbox-api.lumminary.com/v1](https://sandbox-api.lumminary.com/v1)

### Production
website: [lumminary.com](https://lummianary.com)

api-hostname: [api.lumminary.com/v1](https://api.lumminary.com/v1)

# Obtaining credentials
To obtain credentials, you need to register as a Lumminary partner. You can do this by [filling in this form](https://lumminary.com/register-for-connect-with-lumminary).

You will then receive the following:

|Credentials|Description|
|-------|-------|
|Product UUID|Each product you register on the Lumminary platform gets an UUID which will be used to identify that product to the Lumminary API|
|API key|The secret API key associated with the Product UUID|
|Partner UUID|Upon your first registration on the Lumminary platform, you will receive a single Partner UUID, which identifies you as one entity, regardless of product. This identifier is used for the Connect with Lumminary (CWL) functionality.|
|CWL Encryption Key|The CWL encryption key is associated with the Partner UUID and is used to encrypt all communication for the Connect with Lumminary functionality.|

Each product or service needs to have its own product UUID and API key, which means you have to fill in the form for all products and services that require access to Lumminary customer data.  

## Configure the credentials to the Lumminary API Client
The easiest way to set up your credentials is to use an environment file.

For this, you must create a file named `env.json` (but any name will do) in your project directory, which should contain:
```json
{
    "login": <your_product_uuid>,
    "api_key": <your_product_api_key>,
    "role": "role_product",
    "host": <lumminary_api_hostname_endpoint>
}
```

In order to load the Credentials from the env.json, you can use the following code:

#### PHP example:
```php
$credentials = new Lumminary\Client\Credentials();
$credentials->loadJSONCredentials(__DIR__."/env.json");
```

#### Python example:
```python
import lumminary_sdk as lumminary
import os

credentials = lumminary.Credentials()
credentials.load_json_credentials(
    os.path.join(
        os.getcwd(),
        "env.json"
    )
)
```

## Alternative credentials configuration
You also have the option of passing the credentials as constructor parameters when instantiating the `Credentials` class.

#### PHP example:

```php
$credentials = new Lumminary\Client\Credentials(
    <lumminary_api_hostname_endpoint>,
    <your_product_uuid>,
    <your_product_api_key>
);
```

#### Python example:

```python
import lumminary_sdk as lumminary

credentials = lumminary.Credentials(
    uuid = <your_product_uuid>,
    api_key = <your_product_api_key>,
    host = <lumminary_api_hostname_endpoint>,
    role = "role_product"
)
```

## Create an API instance

With the credentials configured and loaded, you can create an API client like so : 

#### PHP example

```php
$apiClient = new Lumminary\Client\ApiClient($credentials);
```

#### Python example

```python
apiClient = lumminary.LumminaryApi(credentials)
```

# Query customers authorizations
An authorization represents permission from a client to access their personal and genetic data. 

There are 2 situations where customers grant you access to their data: 
* when a customer buys your product from the Lumminary DNA App Store
* when a customer clicks on the "Connect with Lumminary" button on your website

Each time either of the above situations happens, our platform creates an authorization UUID. You can reliably assume that if you have an authorization UUID, you automatically have access to all the data needed by your products and services. After you process an authorization you need to flag it as processed; authorizations flagged as processed will no longer be on the list of new authorizations.

There are two ways to obtain the authorization UUID:
* _polling_ - this method allows you to periodically interrogate our API and pulls the list of authorization UUIDs. 
* _webhooks_ (coming soon) - this method allows our API to push the authorization UUIDs into your platform.

## Poll method

To use the polling method, your servers periodically interrogate for new authorization UUIDs. Please keep in mind that authorizations not flagged as processed will always be returned when polling for new authorizations. This means there's a risk of parallel processing the same authorization. To avoid this, you can, for example, consider using locking when processing.

#### A PHP example of using the polling API looks like:

```php
$productAuthorizations = $apiClient->getAuthorizationsQueue(
    $apiClient->getCredentials()->getLogin(),
);

foreach($productAuthorizations as $productAuthorization)
{
    /**
     *  Add your code for processing customer data here
     **/ 
    
    // Flag authorization as processed 
    $apiClient->postProductAuthorization(
        $productAuthorization["productUuid"],
        $productAuthorization["authorizationUuid"]
    );
}
```

#### A Python example of using the polling API looks like:

```python
productAuthorizations = apiClient.get_authorizations_queue(
    apiClient.get_credentials().login
)

for productAuthorization in productAuthorizations:
    #######
    #   Add your code for processing customer data here
    #######

    # Flag authorization as processed
    apiClient.post_product_authorization(
        productAuthorization.product_uuid,
        productAuthorization.authorization_uuid
    )
```

Based on the authorization object obtained previously, we can now query the customer's information (personal details and genetic data).

#### PHP example:
```php
$authorizationMetadata = $apiClient->authorizationMetadata($productAuthorization["authorizationUuid"]);
```

#### Python example:
```python
authorizationMetadata = apiClient.authorization_metadata(productAuthorization.authorization_uuid)
```

#### `authorizationMetadata` object object structure

| Attribute name            | Description                                                                                |
|:-------------------------:|:-------------------------------------------------------------------------------------------|
| `customer`                | The UUID of the customer granting the authorization                                        |
| `product`                 | The UUID of the product that was authorized (your product UUID)                            |
| `authorization`           | The UUID of the granted authorization.                                                     |
| `created_timestamp_utc`   | The unix timestamp in UTC time zone when the customer granted the authorization            |
| `dataset`                 | (present only if requested) The UUID of the dataset authorized by the customer             |
| `customer_email`          | (present only if requested) Customer contact email                                         |
| `customer_name`           | (present only if requested) Customer name                                                  |
| `customer_address`        | (present only if requested) Customer address                                               |

By *present only if requested* we mean this attribute will be returned if at the time of configuring either the "Connect with Lumminary" button or your product, you have explicitly requested for that particular set of data.

# Query customer genetic data
Based on the authorization object obtained previously, now we have authorizationMetadata which contains the customer's information (personal details and genetic data). Let's use this information to extract some customer genetic data.

## Get individual SNPs
Out of all the available SNPs in the dataset, you can only access those for which the customer has previously granted permission.

For example, fetching details for a particular SNP:

#### PHP example:
```php
$rsId = "rs114326054";
$snpDetails = null;

// check to see if you have access to the customer genetic data
if (isset($productAuthorization["scopes"]["dataset"]))
{
    // get SNP information
    $snpDetails = $apiClient->getClientSnp(
        $productAuthorization["clientUuid"],
        $productAuthorization["scopes"]["dataset"],
        $rsId
    );
}
```

#### Python example:
```python
rsId = "rs114326054"
snpDetails = None

# check to see if you have access to the customer genetic data
if hasattr(productAuthorization.scopes, "dataset"):
    # get SNP information
    snpDetails = apiClient.get_client_snp(
        productAuthorization.client_uuid,
        productAuthorization.scopes.dataset,
        rsId
    )
```

##### The snpDetails object will contain these important attributes:

| Attribute name Python     | Attribute name PHP        | Description                                               |
|:-------------------------:|:-------------------------:|:----------------------------------------------------------|
| `snp_id`                  | `snpId`                   | The rsid of the SNP                                       |
| `reference_genome`        |`referenceGenome`          | The reference genome known to be used by the Dataset's source. <br> This impacts the reference allele, location, and based on the dbSNP build, also the SNP's accession |
| `genotyped_alleles`       | `genotypedAlleles`        | The genotype value of the customer's queried SNP. <br><br> If the attribute of this SNP has the `phased` flag set to True, <br>the first items in the lists for all SNPs will be on the same inherited chromosome, <br>and analogous for the second item. <br><br> If the SNP is unphased, the order of the items is irrelevant |
|`phased`                   | `phased`                  | A boolean. True if the SNP is known to be phased, False otherwise |
|`chromosome_accession`     | `chromosomeAccession`     | This is the chromosome accession number where the SNP is located; in the format of NC_00x |
|`location`                 | `location`                | This is the customer's SNP's location on the chromosome |

When trying to access any customer's SNP for which you don't have permission, an `Unauthorized` exception will be raised.

## Get all authorized SNPs

The function below returns all SNPs your product has access to. These are all the SNPs configured as mandatory for your product, as well as all SNPs that are configured as optional and available in the customer's data set. We encourage you to use this option if you need to get all available SNPs, because it is faster than fetching SNP details one by one.

For example, fetching all authorized SNPs:

#### PHP example:
```php
$datasetSnps = null;

// check to see if you have access to the customer genetic data
if (isset($productAuthorization["scopes"]["dataset"]))
{
    // get all authorized SNPs
    $datasetSnps = $apiClient->getClientSnpGroup(
        $productAuthorization["clientUuid"], 
        $productAuthorization["scopes"]["dataset"]
    );
}
```

#### Python example:
```python
datasetSnps = None

# check to see if you have access to the customer genetic data
if hasattr(productAuthorization.scopes, "dataset"):
    # get all authorized SNPs
    datasetSnps = apiClient.get_client_snp_group(
        productAuthorization.client_uuid,
        productAuthorization.scopes.dataset
    )
```

##### The datasetSnps variable will be a list of objects each having the following attributes:

| Attribute name Python     | Attribute name PHP        | Description                                               |
|:-------------------------:|:-------------------------:|:----------------------------------------------------------|
| `snp_id`                  | `snpId`                   | The rsid of the SNP                                       |
| `reference_genome`        |`referenceGenome`          | The reference genome known to be used by the Dataset's source. <br> This impacts the reference allele, location, and based on the dbSNP build, also the SNP's accession |
| `genotyped_alleles`       | `genotypedAlleles`        | The genotype value of the customer's queried SNP. <br><br> If the attribute of this SNP has the `phased` flag set to True, <br>the first items in the lists for all SNPs will be on the same inherited chromosome, <br>and analogous for the second item. <br><br> If the SNP is unphased, the order of the items is irrelevant |
|`phased`                   | `phased`                  | A boolean. True if the SNP is known to be phased, False otherwise |
|`chromosome_accession`     | `chromosomeAccession`     | This is the chromosome accession number where the SNP is located; in the format of NC_00x |
|`location`                 | `location`                | This is the customer's SNP's location on the chromosome |

When trying to access any customer's SNP for which you don't have permission, an `Unauthorized` exception will be raised.

## Get Genes

Along with individual SNPs, you can also get all the authorized SNPs from a particular gene, that are available in the customer's dataset.

Here, by genes, we refer strictly to the genomic region that produces some protein, without considering regulatory or noncoding regions that influence gene expression.

The gene name (known as symbol) can be from either of these two databases - NCBI and Ensembl.

For example, fetching details for a gene symbol:

#### PHP example
```php
$geneSymbol = "C1ORF159";
$geneDetails = null;
// check to see if you have access to the customer genetic data
if (isset($productAuthorization["scopes"]["dataset"]))
{
    // get all authorized SNPs within a gene
    $geneDetails = $apiClient->getClientGene(
        $productAuthorization["clientUuid"],
        $productAuthorization["scopes"]["dataset"],
        $geneSymbol
    );
}
```

#### Python example
```python
geneSymbol = "C1ORF159"
geneDetails = None
# check to see if you have access to the customer genetic data
if hasattr(productAuthorization.scopes, "dataset"):
    # get all authorized SNPs within a gene
    geneDetails = apiClient.get_client_gene(
        productAuthorization.client_uuid,
        productAuthorization.scopes.dataset,
        geneSymbol
    )
```

##### All the geneDetails object attributes are

| Attribute name Python | Attribute name PHP    | Description                                                                              |
|:---------------------:|:---------------------:|:-----------------------------------------------------------------------------------------|
| `molecular_location`  | `molecularLocation`   | An object containing the location of the gene within the chromosome - see below the molecular location object structure  |
| `snps`                | `snps`                | A list of SNP objects present in the gene - see below the SNP object structure           |
| `symbol`              | `symbol`              | The gene's accession string (name)                                                       |

##### Molecular location attributes 

| Attribute name Python     | Attribute name PHP    | Description                                                                              |
|:-------------------------:|:---------------------:|:-----------------------------------------------------------------------------------------|
| `chromosome_accession`    | `chromosomeAccession` | The scaffold/chromosome on which the gene is placed                                      |
| `start`                   | `start`               | The gene's start position on the scaffold                                                |
| `stop`                    | `stop`                | The gene's stop position on the scaffold                                                 |

##### SNP object structure

| Attribute name Python     | Attribute name PHP    | Description                                                                              |
|:-------------------------:|:---------------------:|:-----------------------------------------------------------------------------------------|
| `reference_genome`        | `referenceGenome`     | The reference genome known to be used by the Dataset's source. <br> This impacts the reference allele, location, and based on the dbSNP build, also the SNP's accession|
| `genotyped_alleles`       | `genotypedAlleles`    | The genotype value of the customer's queried SNP. <br><br> If the attribute of this SNP has the `phased` flag set to True, <br>the first items in the lists for all SNPs will be on the same inherited chromosome, <br>and analogous for the second item. <br><br> If the SNP is unphased, the order of the items is irrelevant                                          |
| `phased`                  | `phased`              | A boolean. True if the SNP is known to be phased, False otherwise                         |
| `chromosome_accession`    | `chromosomeAccession` | This is the chromosome accession number where the SNP is located; in the format of NC_00x |
| `location`                | `location`            | This is the customer's SNP's location on the chromosome                                   |

## Get customer genetic data in 23andMe tsv format

If your platform is already compatible with importing data from 23andMe, you can use this specific function to generate data in the 23andMe format - list of rows in tab separated values.

#### PHP example:
```PHP
$authorizationDnaData = $apiClient->authorizationDnaData($productAuthorization["authorizationUuid"]);
```

#### Python example
```python
authorizationDnaData = apiClient.authorization_dna_data(productAuthorization.authorization_uuid)
```

`authorizationDnaData` contains a list of rows in a tsv (tab delimited values)/csv format (23andme-compatible)

# Submit reports

After you finish analysing the customer's genetic data, we need to inform the customer their analysis is complete. To do this, you will notify us using the function below. Finally, the customer will then:

* access their report file through a written electronic document (eg. .pdf or .doc)
* access their report on your website under an account with a username and a password or
* receive a physical product

## How to submit a report file

When you submit such a report file, Lumminary will save this document into the customer's account, from which the customer will then be able to access it directly.

#### PHP example
```php
$fileReport = new \SplFileObject($pathToReport);
$friendlyFileName = "report_file_name"; //optional, give a friendly name to your report file
$apiClient->postAuthorizationResultFile(
   $productAuthorization["productUuid"],
   $productAuthorization["authorizationUuid"],
   $fileReport,
   $friendlyFileName
);
```

#### Python example
```python
pathToReport = <path_to_report_file>
originalFilename = "report_file_name" #optional, give a friendly name to your report file
apiClient.post_authorization_result_file(
   productAuthorization.product_uuid,
   productAuthorization.authorization_uuid,
   pathToReport,
   originalFilename
)
```

If you need to upload multiple files, you have to call the function for each file, one at a time. 

## How to submit a report so the customer can access it on your website

If the customer's results can be accessed on your website, you will need to create a customer account on your platform, generating a user and password which will be sent through the Lumminary API into the customer's Lumminary account. 

In case you don't generate a user and a password for the customer to access their report, use the function below with "null" value to username and password. We recommend you use the URL for customer reports on a dedicated page for reports only, rather than your homepage or some other generic page.

#### PHP example:
```php
$apiClient->postAuthorizationResultCredentials(
   $productAuthorization["productUuid"],
   $productAuthorization["authorizationUuid"],
   <username_generated_by_you>, //optional, default null
   <password_generated_by_you>, //optional, default null
   <report_on_your_website_url> // https://partnerwebsite.com/reports.php
);
```

#### Python example:
```python
apiClient.post_authorization_result_credentials(
   productAuthorization.product_uuid,
   productAuthorization.authorization_uuid,
   <username_generated_by_you>, # optional, default null
   <password_generated_by_you>, # optional, default null
   <report_on_your_website_url> # https://partnerwebsite.com/reports.php
)
```

## How to submit a physical product
In case you only send the customer a physical product and you don't generate any reports, you need to run the function below so we can flag the order as fulfilled and can inform the client.

#### PHP example:
```php
$apiClient->postProductAuthorization(
  $productAuthorization["productUuid"],
  $productAuthorization["authorizationUuid"]
);
```

#### Python example:
```python
apiClient.post_product_authorization(
   productAuthorization.product_uuid,
   productAuthorization.authorization_uuid
)
```

# The Connect with Lumminary button

The "Connect with Lumminary" functionality allows you to get customer details and genetic data from the Lumminary platform for free, anytime you want, for as long the customer grants you access. This functionality offers your customers the option to share their genetic data and other personal information (e.g. name, address, email etc.) stored on the Lumminary platform. Having this button on your website makes it very easy for the customer to share their genetic data with you, as they don't have to download and re-upload their data on your site. The customer always has the option to revoke your access to both their personal details and their genetic data.

**`To protect the customer's privacy, you are not allowed to save their data anywhere. You can, however, always access their data on the Lumminary platform, for as long as they grant you access. If you generate a report based on customer data, you are allowed to save that report on your platform.`**

In order to implement this functionality on your website, this is what you need to know:
* Register your product on the Lumminary platform
* Add the "Connect with Lumminary" button to your website
* Configure your website to retrieve customer data
* Possible errors

## Register your product on the Lumminary platform

If you're new to the Lumminary platform and don't already have any products in the DNA App Store, then you need to register by [filling in this form](https://lumminary.com/register-for-connect-with-lumminary).

You have to fill in the form for all products and services that require access to Lumminary customers' genetic data.  

## Add the "Connect with Lumminary" button to your website

In order to enable the button, you have to include the following script in the `<head>` tag of all the pages where you want to enable the “Connect with Lumminary” button:

```html
<head>
    <script defer src="https://lumminary.com/cwl/cwl.js"></script>
</head>
```

The Javascript creates a CSRF token and attaches it to the button to be transmitted and verified on our servers each time a user clicks on the button. The CSRF token expires every 5 minutes. In case the CSRF token is expired or tempered, the user will be redirected to your website where it's up to you to decide what to do next - reload the page with the button or show the customer an error message.

The cwl.js file is loaded as a deferred resource, which means that it will load after all the webpage code execution has been finished, so it will not have any impact on your website load speed.

### Chose a button colour

There are two type of buttons, so you can pick one that matches your branding. The buttons are SVG images, which means that you can scale them up or down to fit your design, without compromising on image quality. You can do this by changing the image height.

##### a. White button version


<img src="https://lumminary.com/cwl/connect-with-lumminary-white.svg" alt="Lumminary DNA tests" height="50" title="Connect with Lumminary"/>

<br>

```html
<a class="lumminary-connect-btn" data-partner-uuid="<partner-UUID>" data-request="<request>" style="cursor:pointer; text-decoration:none;" href="https://lumminary.com">
   <img src="https://lumminary.com/cwl/connect-with-lumminary-white.svg" alt="Lumminary DNA tests" height="50" title="Connect with Lumminary"/>
</a>
```

##### b.  Black button version

<img src="https://lumminary.com/cwl/connect-with-lumminary-black.svg" alt="Lumminary DNA tests" height="50" title="Connect with Lumminary"/>

<br>

```html
<a class="lumminary-connect-btn" data-partner-uuid="<partner-UUID>" data-request="<request>" style="cursor:pointer; text-decoration:none;" href="https://lumminary.com">
   <img src="https://lumminary.com/cwl/connect-with-lumminary-black.svg" alt="Lumminary DNA tests" height="50" title="Connect with Lumminary"/>
</a>
```

## Button configuration

Each button has 2 attributes which need to be configured:

1. **data-partner-uuid** where you have to add your partner UUID (you have received the partner UUID after filling in the form for product registration). 
2. **data-request** which is a string obtained by encrypting a serialized JSON (you have received the CWL encryption key after filling in the form for product registration).

#### Data-request object

The data-request object has a standard format which needs to be preserved. It is formed of two types of data, some mandatory and some optional. You can use the optional fields to add any metadata or other information for your own use. The data-request object is going to be returned with the response from the authentication without being altered.

##### Mandatory information

The mandatory information is a list of scopes which you ask the client to grant permission for. These scopes are comma delimited, and the possible options are: `address`, `email`, `dataset`.

The scopes _address_, _email_, and _dataset_ can be used in any combination; you must request at least one scope.

| Attribute name    | Description                                         |
|:-----------------:|:----------------------------------------------------|
| `address`         | Requests access to a customer's name and address.   |
| `email`           | Requests access to a customer's email address.      |
| `dataset`         | Requests access to a customer's genetic data        |

#### PHP example:
```php
$objAuthorizationRequest ["scopes"] = "address,dataset,email";
```

#### Python example:
```python
objAuthorizationRequest ["scopes"] = "address,dataset,email"
```

Product UUID is your productUuid for which you ask customer permissions.

#### PHP example:
```php
$objAuthorizationRequest["productUuid"] = $credentials->getLogin();
```

#### Python example:
```python
objAuthorizationRequest["productUuid"] = credentials.get_login()
```

##### Optional information

In the optional part of the object, you can add any useful data, such as customer ID,  session ID, or any parameter which can help you identify the response from Lumminary.

#### PHP example:
```php
$objAuthorizationRequest["customData"] = array();
$objAuthorizationRequest["customData"]["customerId"] = <partner-customer-id>;
$objAuthorizationRequest["customData"]["websiteSession"] = <customer-session>;
$objAuthorizationRequest["customData"]["customData3"] = <Some addional data>;
```

#### Python example:
```python
objAuthorizationRequest["customData"] = {}
objAuthorizationRequest["customData"]["customerId"] = <partner-customer-id>
objAuthorizationRequest["customData"]["websiteSession"] = <customer-session>
objAuthorizationRequest["customData"]["customData3"] = <Some addional data>
```

See below a complete example for a data-request object:

#### PHP example:
```php
$objAuthorizationRequest["scopes"] = "address,dataset,email";
$objAuthorizationRequest["productUuid"] = <product-UUID>;

$objAuthorizationRequest["customData"] = array();
$objAuthorizationRequest["customData"]["customerId"] = <partner-customer-id>;
$objAuthorizationRequest["customData"]["websiteSession"] = <customer-session>;
$objAuthorizationRequest["customData"]["customData3"] = <Some addional data>;
```

#### Python example:
```python
objAuthorizationRequest = {}
objAuthorizationRequest["scopes"] = "address,dataset,email"
objAuthorizationRequest["productUuid"] = <product-UUID>

objAuthorizationRequest["customData"] = {}
objAuthorizationRequest["customData"]["customerId"] = <partner-customer-id>
objAuthorizationRequest["customData"]["websiteSession"] = <customer-session>
objAuthorizationRequest["customData"]["customData3"] = <Some addional data>
```

## Creating the authorization request

The previously generated object (`objAuthorizationRequest`) will now need to be encrypted. In order to be able to encrypt the object and also query the Lumminary API to obtain the customer details and genetic data, you need to have the Lumminary API Client installed. If you haven't done this already, please follow these [steps](#Install-Lumminary-API-Client-andor-Toolkit).

### Add data-request attribute

After you have the Lumminary API Client installed correctly you can use the command below:

#### PHP example:
```php
// You have recieved the CWL encryption key after filling in the form for product registration
$partnerCwlKey = <partner-encryption-key>;
$requestValueEncryptedUrlEncoded = Lumminary\Client\LumminaryApi::cwl_data_request_build(
    $objAuthorizationRequest,
    $partnerCwlKey
);
```

#### Python example:
```python
import lumminary_sdk as lumminary

# You have recieved the CWL encryption key after filling in the form for product
partnerCwlKey = <partner-encryption-key> 
requestValueEncryptedUrlEncoded = lumminary.LumminaryApi.cwl_data_request_build(objAuthorizationRequest, partnerCwlKey)
```

The resulting string should be added in the data-request attribute of the `<a>` tag of the "Connect with Lumminary" button.

### Add data-partner-uuid attribute

Add the data-partner-uuid in the data-partner-uuid attribute of the `<a>` tag of the "Connect with Lumminary" button. You have received the partner UUID after filling in the form for product registration.

An example of a button correctly configured should look like this:

```html
<a class="lumminary-connect-btn" data-partner-uuid="4231-7446-2543-6542" data-request="7LfMX811Als0l%2FAvf84pB7n3mcycnTjgWl1FaVNffdqiOApMn4HAnk0Ux6eatObfYmxf1xPRjo7nBojsL4ImgOL932NK2Ei4VoUXjs9Y%2BcvphI0kxBSblLaeVXNPbJO9LsuNP%2BHJzDBAnZZdAObgYxHH2QDY3VD%2Ff%2FBXKw9WYDdBssAoegMFEJ9GgYutFQ4nTPXAt%2FdWCqoxYbZrYpCj2Pphk9lstc4Ib%2BLNxKiEtNCmVGr6sgmR2lPBwgylTsEX%2FMRCJb6sdQyZBhvSQCMFb0p3%2B9tEwV0%3D" style="cursor:pointer; text-decoration:none;" href="https://lumminary.com">
   <img src="https://lumminary.com/cwl/connect-with-lumminary-white.svg" alt="Lumminary DNA tests" height="50" title="Connect with Lumminary">
</a>
```

## Summary of user interaction

When a customer clicks on the “Connect with Lumminary” button, a pop-up window opens. After they choose which genetic file to share, the pop-up will automatically close and the user will be redirected to your callback URL in the parent window. Your callback URL needs to be predefined in the Lumminary partner portal. 

The GET request from the client to your callback URL will contain two querystring parameters - `request` and `response`:

1. `request` – this is exactly the same request which you previously sent in the data-request field. You can decrypt it with the CWL encryption key which you used to encrypt it.
2. `response` – the response is an urlencoded encrypted serialized JSON object which contains the authorization UUID and the authorization UTC unix timestamp. You will use the authorization UUID to get the customer's data with the Lumminary API Client. The response string is encrypted with your CWL encryption key, the same as the `data-request` parameter. 

In order to decrypt the `response` parameter, you can use the following function:

#### PHP example:
```php
// the entire callback URL, including the querystring parameters
$callbackUrlWithPayload = "https://partnerwebsite.con/callback?request=...&response=...";

$cwlReturnObject = Lumminary\Client\LumminaryApi::cwl_url_query_extract(
    $callbackUrlWithPayload, 
    $partnerCwlKey  
);
```

#### Python example:
```python
// the entire callback URL, including the querystring parameters
callbackUrlWithPayload = "https://partnerwebsite.con/callback?request=...&response=..."

cwlReturnObject = apiClient.cwl_url_query_extract(
    callbackUrlWithPayload,
    partnerCwlKey
)
```

The authorizationPayload will now contain an Object like the example below:

```json
{
    "request": <your-request-parameter-echoed>,
    "response": {
        "authorizationUuid": <cwl-authorization-uuid>,
        "authorizationTimestamp": <cwl-authorization-created-timestamp>
    }
}
```

With the authorization UUID (`authorizationUuid`) you can [query all the customer details](#Query-customer-genetic-data) from the Lumminary platform.

## Possible errors

When an error occurs, the customer is redirected back to your callback URL. The redirect contains two querystring parameters - `request` and `response` - exactly like a regular response, but the `response` parameter contains an error object (see below) instead of an authorization object.

#### PHP example:
```php
// the entire callback url, including the querystring parameters
$callbackUrlWithPayload = "https://partnerwebsite.con/callback?request=...&response=...";

$cwlReturnObject = Lumminary\Client\LumminaryApi::cwl_url_query_extract(
    $callbackUrlWithPayload, 
    $partnerCwlKey  
);
```

#### Python example:
```python
# the entire callback url, including the querystring parameters
callbackUrlWithPayload = "https://partnerwebsite.con/callback?request=...&response=..."

cwlReturnObject = apiClient.cwl_url_query_extract(
    callbackUrlWithPayload,
    partnerCwlKey
)
```

Example of a return object (`cwlReturnObject`) containing an error message:

```json
{
    "request": <your-request-parameter-echoed>,
    "response": {
        "error": {
            "id": <error id>,
            "message": <error message>
        }
    }
}
```

| Error Id          | Error Message                                                                                |
|:-----------------:|:---------------------------------------------------------------------------------------------|
| 1                 | Invalid Security Token                                                                       |
| 2                 | Invalid Access Scopes                                                                        |
| 3                 | Customer refuses request (this happens when the customer cancels instead of granting access) |


## Authorization

Supported authorization schemes:
- API Key
## Actions

### General-purpose authentication

> If 2FA is enabled, the 2FA token is validated along with the username/password pair.<br/>
> Otherwise, the 2FA token, even if provided, is ignored.<br/>
> <br/>
> ## Note:<br/>
> * A fresh and not previously used 2FA token should be passed, otherwise authentication will fail.<br/>
> * The JWT tokens returned should be passed to any resource that requires authentication, in the Authentication header, in the format `Bearer: your-token-here`<br/>
> * Only JWT authentication tokens are provided (no refresh tokens). These tokens are valid for 30 seconds from the moment they were issued

*Tags:* `Lumminary API Spec`

#### Input Parameters
* `X-Fields` - _optional_ - An optional fields mask

### Get gene by symbol

> Gets A gene by its symbol, which can be found by querying the reference/ resource.<br/>
> <br/>
> Will return a 404 if a gene exists as a reference, but its genomic coordinates intersect no SNPs in the dataset

*Tags:* `Lumminary API Spec`

#### Input Parameters
* `X-Fields` - _optional_ - An optional fields mask
* `datasetId` - _required_ - The UUID of one of the client's dataset
* `geneSymbol` - _required_ - The symbol of a gene to be fetched

### get_client_snp_group

*Tags:* `Lumminary API Spec`

#### Input Parameters
* `X-Fields` - _optional_ - An optional fields mask
* `datasetId` - _required_ - The UUID of one of the client's dataset

### Get a large group of SNPs

> SNPs that are not present in the dataset are ignored, 404 is returned only if the dataset/client does not exist

*Tags:* `Lumminary API Spec`

#### Input Parameters
* `X-Fields` - _optional_ - An optional fields mask
* `datasetId` - _required_ - The UUID of one of the client's dataset

### Get SNP information

> Gets SNP information, as provided by the dataset.<br/>
> <br/>
> If fetching this as an product, the client must have already granted access to the snip (see the 'products' group)

*Tags:* `Lumminary API Spec`

#### Input Parameters
* `X-Fields` - _optional_ - An optional fields mask
* `datasetId` - _required_ - The UUID of one of the client's dataset
* `snpId` - _required_ - The rsId of a SNP to be fetched

### Get product details

*Tags:* `Lumminary API Spec`

#### Input Parameters
* `X-Fields` - _optional_ - An optional fields mask

### get_authorizations_queue

*Tags:* `Lumminary API Spec`

#### Input Parameters
* `seq_num_start` - _optional_ - The first sequence number from which to fetch (the sequence number of the last processed authorization)
* `X-Fields` - _optional_ - An optional fields mask

### get_product_authorization

*Tags:* `Lumminary API Spec`

#### Input Parameters
* `X-Fields` - _optional_ - An optional fields mask
* `authorizationId` - _required_ - The UUID of the authorization

### Singnal that processing is complete, without uploading any result

*Tags:* `Lumminary API Spec`

#### Input Parameters
* `productId` - _required_ - The UUID of the product
* `authorizationId` - _required_ - The UUID of the authorization

### Provide a result for the authorization

> These can be log-in credentials for a website where the result is available

*Tags:* `Lumminary API Spec`

#### Input Parameters
* `productId` - _required_ - The UUID of the product
* `authorizationId` - _required_ - The UUID of the authorization

### Provide a file result to the authorization, e

> g. a pdf report

*Tags:* `Lumminary API Spec`

#### Input Parameters
* `original_filename` - _optional_ - Optional original filename for the report. If not provided, the filename of uploaded file will be used
* `authorizationId` - _required_ - The UUID of the authorization

### Generic gene information

*Tags:* `Lumminary API Spec`

#### Input Parameters
* `dbsnp_build` - _optional_ - The dbSNP build for which to consider snps belonging to the gene. Defaults to 149
* `reference_genome` - _optional_ - The reference genome for which gene annotations will be returned. Defaults to GRCh37p13
* `X-Fields` - _optional_ - An optional fields mask

### Reference genome builds

> Lists reference genome builds that are available

*Tags:* `Lumminary API Spec`

#### Input Parameters
* `X-Fields` - _optional_ - An optional fields mask

### Reference genome metadata

*Tags:* `Lumminary API Spec`

#### Input Parameters
* `X-Fields` - _optional_ - An optional fields mask

### Sequence for a region of the reference genome

> Fetch a closed interval of nucleotides on a given chromosome. Includes start and stop positions

*Tags:* `Lumminary API Spec`

#### Input Parameters
* `range_start` - _required_ - Location on the chromosome
* `range_stop` - _required_ - Location on the chromosome
* `X-Fields` - _optional_ - An optional fields mask

### Reference SNP data

> Get reference SNP information, from dbSNP

*Tags:* `Lumminary API Spec`

#### Input Parameters
* `dbsnp_version` - _optional_ - The dbSNP build. Defaults to 149
* `grch_version` - _optional_ - The GRCh build on which to place snips. Defaults to GRCh37p13
* `X-Fields` - _optional_ - An optional fields mask

## License

**flow**ground :- Telekom iPaaS / lumminary-com-connector<br/>
Copyright © 2019, [Deutsche Telekom AG](https://www.telekom.de)<br/>
contact: flowground@telekom.de

All files of this connector are licensed under the Apache 2.0 License. For details
see the file LICENSE on the toplevel directory.
