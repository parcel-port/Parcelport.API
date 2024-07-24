#  Get available shipping rates

Allows third-party systems to get a list of applicable shipping rate based on
the passed sender's address, recipient's address and parcel(s)’ dimensions.

- URL: /api/consignment/quote
- Http Method: POST

*This API can only be called after the API authentication is approved (the correct
auth string has been passed).

## Required Parameters:
*IsSignature* - [Optional, default is '*0*', '*1*' if signature required]

*Parcels* - [Required, a list of parcel]
- **PackageId** - [Optional, default is '**', see the following table of *Zappy satchel list* ]
- **Length** - [Required if not using satchel, length of the parcel cm]
- **Width** - [Required if not using satchel, width of the parcel cm]
- **Height** - [Required if not using satchel, height of the parcel cm]
- **Weight** - [Required, weight of the parcel Kg]
- **ItemRef** - [Optional reference of the item]
- **ItemNo** - [Required index of the item]

*RecipientAddress*
- **AddressStreet** [Require, unit number + street number + street name]
- **AddressState** [Require, state]
- **AddressSuburb** [Require, suburb]
- **AddressCity** [Require, city]
- **AddressPostcode** [Require, postcode]
- **AddressCountry** [Require, country]
- **ContactName** [Require, contact name]
- **AddressUnit** [Optional, unit number]
- **AddressBuilding** [Optional, building]
- **Email** [Require, sender email]
- **Phone** [Optional, contact phone]
- **CompanyName** [Optional, company name] 
- **Instruction** [Optional, instruction]
- **IsCommercialAddress** [Optional, is commercial address, default is '*0*', '*1*' if commercial address]

*SenderAddress*
- **AddressStreet** [Require, unit number + street number + street name]
- **AddressState** [Require, state]
- **AddressSuburb** [Require, suburb]
- **AddressCity** [Require, city]
- **AddressPostcode** [Require, postcode]
- **AddressCountry** [Require, country]
- **ContactName** [Require, contact name]
- **AddressUnit** [Optional, unit number]
- **AddressBuilding** [Optional, building]
- **Email** [Require, sender email]
- **Phone** [Optional, contact phone]
- **CompanyName** [Optional, company name] 
- **Instruction** [Optional, instruction]
- **IsCommercialAddress** [Optional, is commercial address, default is '*0*', '*1*' if commercial address]

*Zappy satchel list*
<table>
  <tr>
    <th>Satchel name</th>
    <th>code</th>
  </tr>
  <tr>
    <td>Satchel 185x280 A5 Size</td>
    <td>a5</td>
  </tr>
  <tr>
    <td>Satchel 250x325 A4 Size</td>
    <td>a4</td>
  </tr>
  <tr>
    <td>Satchel 325x440 A3 Size</td>
    <td>a3</td>
  </tr>
  <tr>
    <td>Satchel 450x610 A2 Size</td>
    <td>a2</td>
  </tr>
</table>

## Example
**Request Headers**
```
Content-Type: application/json;
```

**Request Authorization**
```
Bearer:eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJhM2E5NzIxYy1iMjQ0LTQwMjMtYWVhNS0xNmYzYzliMzk2OTkiLCJuYW1lIjoiSVQgVGVzdCIsImVtYWlsIjoidGVzdEBzaGlwemFwcHkuY29tIiwicGhvbmVfbnVtYmVyIjoiMDAwMDAwMDAiLCJodHRwOi8vc2NoZW1hcy5taWNyb3NvZnQuY29tL3dzLzIwMDgvMDYvaWRlbnRpdHkvY2xhaW1zL3JvbGUiOiJVc2VyIiwidXNlcklkIjoiMDAyZmVhZDQ2ZmIxNDY5YmI1NGJmMjlhNDI0NDM4MmUiLCJjb21wYW55SWQiOiIyIiwiZXhwIjoxNzIxNzgzNDQ0LCJpYXQiOjE3MjE2OTcwNDQsIm5iZiI6MTcyMTY5NzA0NH0.0w7uP9k3Z_9y0va3ltxTULGH_yhSqZ9kTGEK1ofqgAA
```

**Request Body**
``` json
{
    "SenderAddress": {
        "AddressUnit": "",
        "AddressBuilding": "",
        "AddressStreet": "12 Pitt Street",
        "ContactName": "Kevin",
        "CompanyName": "Zapy",
        "AddressSuburb": "Parramatta",
        "AddressCity": "Parramatta",
        "AddressState": "NSW",
        "AddressCountry": "AU",
        "AddressPostcode": "2150",
        "Phone": "2102607358",
        "Email": "kevin.h@shipzappy.com",
        "Instruction": "",
        "IsCommercialAddress": "0"
    },
    "RecipientAddress": {
        "AddressUnit": "",
        "AddressBuilding": "",
        "AddressStreet": "450 Nepean Highway",
        "ContactName": "to",
        "CompanyName": "",
        "AddressSuburb": "Mount Martha",
        "AddressCity": "Mount Martha",
        "AddressState": "VIC",
        "AddressCountry": "AU",
        "AddressPostcode": "3934",
        "Phone": "123456",
        "Email": "www@qq.com",
        "Instruction": "123ert",
        "IsCommercialAddress": "0"
    },
    "Parcels": [
        {
            "ItemNo": 1,
            "Length": "1",
            "Width": "1",
            "Height": "1",
            "Weight": "1",
            "ItemRef": "123"
        }
    ],
    "IsSignature": "0"
}
```
**Responses**  
A JSON encoded string contains all valid shipping rates with a price and a shipping method id.  
The rates will be expired in 15 minutes.  
Choose a carrier from the list of rate and [create a consignment](Consignment/README.md) using it's QuoteRequestUuid.

``` json
{
    "IsSuccess": true,
    "Data": {
        "QuoteRequestUuid": "09bd6130574a42e8b3b2f88ab3d16fc1",
        "DateExpired": "2024-07-24T07:43:56.2078857Z",
        "ErrorMessage": null,
        "CarrierErrors": {
            "cp": [
                "Sender address cannot be picked up."
            ]
        },
        "Quotes": [
            {
                "CarrierId": "ft",
                "CarrierMethodId": "ft_freight",
                "PricingPlanId": 1,
                "TotalExlGst": 7.21,
                "GstTotal": 0.73,
                "AddOns": [
                    {
                        "CarrierMethodId": "ft_freight",
                        "PriceGst": 0.66,
                        "PriceExlGst": 6.57,
                        "CostGst": 0.66,
                        "CostExlGst": 6.57,
                        "IsSelected": true,
                        "CarrierMethodName": "Freight",
                        "CarrierMethodDescription": "Freight",
                        "CarrierMethodCode": "",
                        "TaxRate": 0.100,
                        "FormulaInstanceId": 1,
                        "FormulaParamA": 1.000000,
                        "FormulaParamB": 0.000000
                    },
                    {
                        "CarrierMethodId": "ft_ruc",
                        "PriceGst": 0.07,
                        "PriceExlGst": 0.64,
                        "CostGst": 0.07,
                        "CostExlGst": 0.64,
                        "IsSelected": true,
                        "CarrierMethodName": "Fuel Surcharge",
                        "CarrierMethodDescription": "",
                        "CarrierMethodCode": "",
                        "TaxRate": 0.100,
                        "FormulaInstanceId": 1,
                        "FormulaParamA": 1.000000,
                        "FormulaParamB": 0.000000
                    }
                ],
                "QuoteItems": [],
                "CarrierName": null,
                "CarrierLogo": null
            }
        ]
    }
}
```

***
