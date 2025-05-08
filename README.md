# ABAP Developer Challenge
[![REUSE status](
https://api.reuse.software/badge/github.com/SAP-samples/sap-community-abap-developer-challenge)](https://api.reuse.software/info/github.com/SAP-samples/sap-community-abap-developer-challenge)

# Containing Files

1. The LICENSE file:
In most cases, the license for SAP sample projects is `Apache 2.0`.

2. The .reuse/dep5 file: 
The [Reuse Tool](https://reuse.software/) must be used for your samples project. You can find the .reuse/dep5 in the project initial. Please replace the parts inside the single angle quotation marks < > by the specific information for your repository.

3. The README.md file (this file):
Please edit this file as it is the primary description file for your project. You can find some placeholder titles for sections below.

# 2025 ABAP Developer Challenge Base Data Model

The pre requisite to this challenge is to create the base model for Travel Data and be ready for extending them at each step to complete the challenge. Here we will be creating a SAP-like database table with an existing include structure with 1 field. You will also create a data generator class to fill data to this table. Next you will create a CDS view entity for this database table. The steps to create each of these objects are mentioned below. Follow the steps carefully and do a data preview after each step to make sure you have enough demo data to proceed. 

Note:- Please make sure to replace 'XXX' with a unique alphanumeric character of your choice for all the objects that you create.

So let us start...

1. Create a database table to store the Travel data and copy the following code.

   This Travel table contains Travel ID, Total Price and Currency Code.

<pre lang="ABAP">

@EndUserText.label : 'Travel data for ABAP Challenge'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
define table ztravel_123 {
 
  key client    : abap.clnt not null;
  key travel_id : /dmo/travel_id not null;
  @Semantics.amount.currencyCode : 'ztravel_xxx.currency_code'
  total_price   : /dmo/total_price;
  currency_code : /dmo/currency_code;
  include ztravel_struc_123;
 
}

</pre>

2.  Now create the structure included in the code above which will be an extension of the table.
    ZTRAVEL_123.
    
<pre lang="ABAP">
@EndUserText.label : 'Structure of Travel Data'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
@AbapCatalog.enhancement.fieldSuffix : 'ZAC'
@AbapCatalog.enhancement.quotaMaximumFields : 350
@AbapCatalog.enhancement.quotaMaximumBytes : 3500
define structure ztravel_struc_123 {
 
  description : /dmo/description;
 
}
</pre> 

Activate the structure first and then the table to complete the table creation.


3.  Next we will create data generator class to fill data to this table by fetching data from 
    /DMO/TRAVEL table.
    

<pre lang="ABAP">

CLASS ztravel_fill_data_123 DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC .
 
  PUBLIC SECTION.
    INTERFACES if_oo_adt_classrun.
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.

 
CLASS ztravel_fill_data_xxx IMPLEMENTATION.
 
  METHOD if_oo_adt_classrun~main.
 
   INSERT ztravel_123 FROM ( SELECT FROM /dmo/travel FIELDS travel_id, total_price, currency_code, description ).
 
  ENDMETHOD.
ENDCLASS.
</pre>

Activate and execute the class and do a data preview of the table to make sure you have enough demo data to proceed.

Note: For standard SAP-delivered tables, you will not be able to do a Data Preview from ADT.


4.  Create a CDS view entity based on the database table above.

<pre lang="ABAP">
@AbapCatalog.viewEnhancementCategory: [#PROJECTION_LIST]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Travel View Entity'
@Metadata.ignorePropagatedAnnotations: true
@AbapCatalog.extensibility: {
  extensible: true,
  elementSuffix: 'ZAC',
  quota: {
    maximumFields: 500,
    maximumBytes: 5000
  },
  dataSources: [ '_Travel' ]
}
define view entity ZITRAVEL_123
  as select from ztravel_xxx as _Travel
{
  key travel_id     as TravelId,
      description   as Description,
      @Semantics.amount.currencyCode: 'CurrencyCode'
      total_price   as TotalPrice,
      currency_code as CurrencyCode
}
</pre>

Activate the view and do a data preview of the view entity and make sure you see data in all the columns available.

Now you are ready to start the challenge. Proceed with the next steps in the blogpost to unlock question for each step.


# 2024 ABAP Developer Challenge Solution
<!-- Please include descriptive title -->

<!--- Register repository https://api.reuse.software/register, then add REUSE badge:
[![REUSE status](https://api.reuse.software/badge/github.com/SAP-samples/REPO-NAME)](https://api.reuse.software/info/github.com/SAP-samples/REPO-NAME)
-->

## Description
This has the solution for the ABAP Developer Challenge.

## Solution
All the Solution is available in the folder 'Solution' : https://github.com/SAP-samples/sap-community-abap-developer-challenge/tree/main/Solution


## How to obtain support
[Create an issue](https://github.com/SAP-samples/<repository-name>/issues) in this repository if you find a bug or have questions about the content.
 
For additional support, [ask a question in SAP Community](https://answers.sap.com/questions/ask.html).

## Contributing
If you wish to contribute code, offer fixes or improvements, please send a pull request. Due to legal reasons, contributors will be asked to accept a DCO when they create the first pull request to this project. This happens in an automated fashion during the submission process. SAP uses [the standard DCO text of the Linux Foundation](https://developercertificate.org/).

## License
Copyright (c) 2024 SAP SE or an SAP affiliate company. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSE) file.
