
<!-- README.md is generated from README.Rmd. Please edit that file -->
<!-- You need to render `README.Rmd` to keep `README.md` up-to-date. -->
<!-- use`devtools::build_readme()` for this.  -->

# kitems

<!-- badges: start -->
<!-- badges: end -->

The goal of kitems is to provide a framework to manage data.frame like
items and a set of tools to implement it within a shiny web application.
For that reason, it is delivered as a shiny module.

## Installation

You can install the development version of kitems from
[GitHub](https://github.com/thekangaroofactory/kitems) with:

``` r
# install.packages("devtools")
devtools::install_github("thekangaroofactory/kitems")
```

## Demo

A basic shiny demo app which shows you how to use the package is
delivered and can be run:

## Framework Specifications

The kitems framework is based on a two main notions: data model and
items.

## Data Model

The data model contains the specifications of the items you want to
manage. It includes:

- attribute names,
- attribute types,
- default values,
- default functions,
- filter,
- skip.

Attribute names and types are obvious. Supported data types are:

- numeric,
- integer,
- double,
- logical,
- character,
- factor,
- Date,
- POSIXct,
- POSIXlt.

## Items

Items is a data.frame containing the data to be managed.

## Reactive Values

kitems strongly relies on Shiny (shiny and shinydashboard packages are
set as ‘depends’ dependencies). It provides reactive values (accessible
from the r object passed as an argument of the module server):

- r\[\[data_model\]\]
- r\[[items](#items)\]
- r\[\[trigger_add\]\]
- r\[\[trigger_update\]\]
- r\[\[trigger_delete\]\]
- r\[\[trigger_save\]\]

> \[!CAUTION\] Data model and items reactive values should be handled
> with caution as updating there values will trigger auto save (for
> items, if autosave = TRUE).

### Data Model

``` r
library(kitems)
#> Le chargement a nécessité le package : shiny
#> Le chargement a nécessité le package : shinydashboard
#> 
#> Attachement du package : 'shinydashboard'
#> L'objet suivant est masqué depuis 'package:graphics':
#> 
#>     box
#> 
#> Attachement du package : 'kitems'
#> L'objet suivant est masqué depuis 'package:shiny':
#> 
#>     runExample

# -- define module id
id <- "mydata"

# -- get data model reactiveValue name
data_model <- dm_name(id)
```

The data model for this id can be accessed **in a reactive context** by
using: r\[\[data_model\]\]

### Items

### Triggers

Triggers are basically reactiveValues stored in the r object that can be
used to communicate actions to the module server.

Their name is built with paste0(id, “\_trigger_name”), with
\[trigger_name\] being: \* trigger_add: add an item to the item list \*
trigger_update \<- update an item from the item list \* trigger_delete
\<- delete an item from the list \* trigger_save \<- save the item list
(if autosave is turned off)

## Item creation

It’s recommended to use item_create() to create the item to be added to
the list:

id \<- “my_data”

r_data_model \<- dm_name(id) trigger_add \<- trigger_add_name(id)

input_values \<- data.frame(id = 1, text = “demo”)

item \<- item_create(values = input_values, data.model =
r[\[r_data_model\]]()) r[\[trigger_add\]](item)

Note: if autosave has been turned off, r\[\[trigger_save\]\] should be
used to make item changes persistent.

## Item update

It’s recommended to use item_create() to create a new item to replace
the one in the list:

id \<- “my_data”

r_data_model \<- dm_name(id) trigger_update \<- trigger_update_name(id)

input_values \<- data.frame(id = 1, text = “demo update”)

item \<- item_create(values = input_values, data.model =
r[\[r_data_model\]]()) r[\[trigger_update\]](item)

## Item delete

To delete an item, just pass it’s id to the trigger:

id \<- “my_data”

trigger_delete \<- trigger_delete_name(id)

item_id \<- 1704961867683 r[\[trigger_delete\]](item_id)

------------------------------------------------------------------------

## Inputs

### date_slider_INPUT

If the data model has an attribute named ‘date’, a date sliderInput will
be created automatically to enable date filtering.

This sliderInput will trigger a filter on the items to be displayed in
the filtered view.

If not implemented in your application’s UI, then no date filtered is
applied by default.

### Buttons

create item update item : single selection delete item : support multi
selection
