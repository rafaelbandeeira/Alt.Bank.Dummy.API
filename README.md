# Approach Breakdown

baseUrl: dummy.restapiexample.com/api/  
Project risks: not applicable as there is no client / acceptance criteria for the API.  

Identified test process risks:   
* Documentation not available/incomplete  
* API behaviour is static, which blocks most testing techniques  
* Low server capacity, which makes running multiple tests difficult (returns 429, too many requests)  

Weapons of choice (tools):  
* JsonSchema Tool: generates a JSON schema to compare response/request bodies against specified JSON. Available here: https://jsonschema.net/  
* Postman: open-source tool for API manipulation. The lack of able time shortened the tool options. Among the choices, Postman was the simplest and could provide a semi-automated approach. My preferred choice was REST-Assured, but the risk of not meeting the deadline would be increased.  
Total number of endpoints: 5
Total number of methods: 4
    
# Test Strategies

Risk-based testing: not applicable as no risks were identified.  
Code coverage: not applicable, the API source-code is not available for debugging.  
Compatibility test: not applicable, the API has only one version.  
Pact testing: possible, but short-handed due to the lack of documentation. The JSON samples provided were used to guide this approach.  
Test coverage model: possible, but short-handed due to API capabilities.

## Test Plan
Selected approaches: strategies applied to the API testing  
* Test coverage model: specifies multiple black-box testing layers to test an API and its various aspects. The first layer is the weakest and the last layer is the strongest, meaning the complexity and coverage increases as you go down the layers.  
* Pact test: Using a pre-defined model, it's possible to compare if the output matches the contract, increasing reliability between consumer and provider.
   
## Test Coverage Model
The counting is done per method/endpoint, not per different samples.
The test cases number are the sum of all the assertions, not requests (they were separated in multiple folders for clarity).

|            Coverage            |  Possible  |  Covered  |  Total # of tests  |
|:-------------------------------|:----------:|:---------:|:------------------:|
|1. Paths                        |      5     |     5     |         5          |
|2. Operations*                  |      5     |     5     |         5          | 
|3. Content-type                 |      7     |     7     |         12         |
|4. Status code classes          |      10    |     10    |         22         |
|5. Parameters*                  |      3     |     3     |         22         |
|6. Status codes*                |      13    |     13    |         25         |
|7. Response body properties***  |      5     |     5     |         30         |
|8. Operation flows**            |      --    |     --    |         --         |

\* These layers were either partially or completely covered by/in another layer. The ones which were completely covered had their folders omitted.  
\** It is not possible to exercise this layer due to API limitations and no documentation specifying the consumer flow.  
\*** The Pact test was covered in this layer.

## Test Suite Structure
A folder was created for each layer and the ones that were covered by other layers had their folders omitted. In the environment variables section, you will find two different variables:  
* baseUrl: to simplify the access to the URI  
* jsonSchema: I used the pre-test section to specify the JsonSchema of each request and add it to an environment variable, so a the test ran in the folder could have access to that data without writting the same script in multiple requests.
    
This test suite uses Newman for scripted runs and HTMLExtra to generate the report. To run the two together, run `newman run collection.json -r htmlextra` in the project folder.  
Make sure Newman is installed by running the command `npm install -g newman` and `npm install -g newman-reporter-htmlextra` for the HTMLExtra.  

To access the report generated when the test suite was first run, copy the project to your machine and open the html file in "folder/newman/Alt.Bank Challenge-2020-11-30-15-56-01-687-0".
