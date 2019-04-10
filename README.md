# U.S. Congressional Districts

This package contains GeoJSON for the U.S. Congressional Districts for the 116th Congress, based on the U.S. Census Bureau data for that year, that can be uploaded in the form of a Salesforce Einstein Analytics custom map. It also contains a custom field on the Contact object to contain the district information as well as a validation rule for that field to ensure that data stored there will properly index the map.

Finally, I have included a [Mockaroo](http://mockaroo.com) schema and dataset to assist the generation of dummy state and district information for demos.


## Using Mockaroo to Generate Dummy Demo Data for the Map

The custom map indexes districts by <*Two-Character State Code*>-<*Two-Digit District Number*>, for example, "VA-02" or "TX-43". In order to make sure that we don't generate invalid districts (for example, "VA-53" -- Virginia only has 11 districts), it is necessary to upload a [US Districts by State](/mockaroo/US%20Districts%20by%20State.csv) dataset into Mockaroo which contains a list of the states and the total number of districts each has.

From there, it's easy in Mockaroo to generate random state abbreviations and then generate random districts based on those states using the US Districts by State dataset:

![Schema Snippet](/images/Mockaroo_Schema.png)

You can upload the [schema snippet](/mockaroo/US%20District%20Generator.schema.json) shown above into Mockaroo to help get you started.

The relevant formula, once `state` has been generated, is given by:
```
state + "-" + ("%02d" % random(1, from_dataset("US Districts by State", "Number_of_CDs", State:state).to_i))
```
- `from_dataset` pulls in the number of Congressional Districts from the `US Districts by State` dataset you uploaded earlier, using `state` as a primary key.
- `.to_i` converts the number string to an integer.
- `random` generates a random number between 1 and the total number of districts the state has.
- `"%02d"` formats the number to have a leading 0 in case the random number generated is less than 10.
- the rest just concatenates the `state` and a hyphen and the number.


## How to Deploy to Your Org

Simply click the button below and log into your org when requested:

<a href="https://githubsfdeploy.herokuapp.com">
  <img alt="Deploy to Salesforce"
       src="https://raw.githubusercontent.com/afawcett/githubsfdeploy/master/src/main/webapp/resources/img/deploy.png">
</a>