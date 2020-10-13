# Loan Pre-qualification

This project is based on the Loan Pre-qualification model used by Bruce Silver in his [DMN Method & Style book](https://www.amazon.com/Dmn-Method-Style-2nd-Pracitioners/dp/0982368178).

## Loan Prequalification (main DRD)
![Loan Prequalification.dmn](./LoanPrequalification.dmn.png)

---

### Adjusted Loan Rate Decision Service (reusable Decision Service DRD)
![Adjusted Loan Rate.dmn](Adjusted%20Loan%20Rate.dmn.png)

You can import the project on [Decision Central (RHDM or RHPAM)](https://www.redhat.com/en/technologies/jboss-middleware/decision-manager).

You can also use VSCode with [Kogito Extension](https://marketplace.visualstudio.com/items?itemName=kie-group.vscode-extension-kogito-bundle) which comes with DMN and BPMN editors.

## Testing the decision service through Kie Server

after deploying you model to a kie server runtime instance you can test it via Rest API:

```bash
curl --location --request POST 'http://localhost:8080/kie-server/services/rest/server/containers/DMNLoanPrequalification/dmn' \
--header 'Accept: application/json' \
--header 'X-KIE-ContentType: application/json' \
--header 'Authorization: Basic <your kie-server credentials base64 encoded here>' \
--header 'Content-Type: application/json' \
--data-raw '{
  "model-namespace": "https://kiegroup.org/dmn/_556FD415-D569-4331-89EF-F917CE6C6267",
  "model-name": "LoanPrequalification",
  "decision-name" : [ ],
  "dmn-context" : 
    {
        "LTV ratio" : 75,
        "Best rate" : 0.2 ,
        "Down Payment": 40000,
        "Purchase Price": 300000,
        "Credit Score": 750, 
        "Monthly Income": 8000
    }
}'
```

you should a get a response like this:

```json
{
    "type": "SUCCESS",
    "msg": "OK from container 'DMNLoanPrequalification'",
    "result": {
        "dmn-evaluation-result": {
            "messages": [],
            "model-namespace": "https://kiegroup.org/dmn/_556FD415-D569-4331-89EF-F917CE6C6267",
            "model-name": "LoanPrequalification",
            "decision-name": [],
            "dmn-context": {
                "Affordability Category": "Affordable",
                "LTV ratio": 75,
                "Loan Prequalification": "Likely approved",
                "Monthly Income": 8000,
                "Tax and Insurance Payment": 400.0000,
                "DTI pct": 14.34541882527206301243257542171532,
                "Loan Payment": 747.6335060217650409946060337372257,
                "services": {
                    "Adjusted Loan Rate": "function Adjusted Loan Rate( p_Best rate, p_Credit score, p_LTV )"
                },
                "Best rate": 0.2,
                "Loan Amortization Formula": "function Loan Amortization Formula( p, r, n )",
                "Housing Expense": 1147.633506021765040994606033737226,
                "Loan rate pct": 0.23125,
                "Down Payment": 40000,
                "Loan Amount": 260000,
                "Purchase Price": 300000,
                "Credit Score": 750
            },
            "decision-results": {
                "_DBBD128A-7AE5-48CB-9BEB-E898F17B3DD1": {
                    "messages": [],
                    "decision-id": "_DBBD128A-7AE5-48CB-9BEB-E898F17B3DD1",
                    "decision-name": "Loan Amount",
                    "result": 260000,
                    "status": "SUCCEEDED"
                },
                "_0DB98860-50C4-49C8-A9D0-B77DEB785343": {
                    "messages": [],
                    "decision-id": "_0DB98860-50C4-49C8-A9D0-B77DEB785343",
                    "decision-name": "Tax and Insurance Payment",
                    "result": 400.0000,
                    "status": "SUCCEEDED"
                },
                "_3EB49B27-6649-47BC-90B7-43BD4111C8A4": {
                    "messages": [],
                    "decision-id": "_3EB49B27-6649-47BC-90B7-43BD4111C8A4",
                    "decision-name": "Loan rate pct",
                    "result": 0.23125,
                    "status": "SUCCEEDED"
                },
                "_B9FFD990-B340-4B59-9BA5-C18731224B8F": {
                    "messages": [],
                    "decision-id": "_B9FFD990-B340-4B59-9BA5-C18731224B8F",
                    "decision-name": "Loan Payment",
                    "result": 747.6335060217650409946060337372257,
                    "status": "SUCCEEDED"
                },
                "_531AFF56-9BF6-4F84-A21F-6EF9936EF748": {
                    "messages": [],
                    "decision-id": "_531AFF56-9BF6-4F84-A21F-6EF9936EF748",
                    "decision-name": "Housing Expense",
                    "result": 1147.633506021765040994606033737226,
                    "status": "SUCCEEDED"
                },
                "_F4585B76-03C2-4744-AE67-9273D60390BE": {
                    "messages": [],
                    "decision-id": "_F4585B76-03C2-4744-AE67-9273D60390BE",
                    "decision-name": "Affordability Category",
                    "result": "Affordable",
                    "status": "SUCCEEDED"
                },
                "_9BE479AF-AB00-49D3-960F-ADF3C1FF3CB6": {
                    "messages": [],
                    "decision-id": "_9BE479AF-AB00-49D3-960F-ADF3C1FF3CB6",
                    "decision-name": "DTI pct",
                    "result": 14.34541882527206301243257542171532,
                    "status": "SUCCEEDED"
                },
                "_7067B61C-7446-4104-BA04-7207B2893FE2": {
                    "messages": [],
                    "decision-id": "_7067B61C-7446-4104-BA04-7207B2893FE2",
                    "decision-name": "Loan Prequalification",
                    "result": "Likely approved",
                    "status": "SUCCEEDED"
                }
            }
        }
    }
}
```