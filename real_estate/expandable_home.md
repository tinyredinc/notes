Marketing Plan for Expandable Homes

## Business Model

### [Retail]
- Target Audience
- Estimated Price Range per Unit
- Service & Product Options
- Discounts for Multiple Purchases
- Key Selling Points

### [Wholesale]
- Target Audience
	- Resort Owner
	- Real Estate Developer/Investor
	- Trailer Park Owner
	- Affordable Projects from GOV
- Minimum Purchase Quantity
	- At least 10 or 10+
- Estimated Price Range per Unit
	- Negotiable base on terms and scales
- Service & Product Options
	- Demand driven customization
- Key Selling Points
	- Matching 

## Marketing Materials
- Brand Name and Logo
- 3D Product Models (packed/expanded/perspective)
- Brochure/Flyer/Card
- Presentation (PPT)
- Promotional Video
- Landing Page/Website

## Lead Registration System
- Categorization
	- Retail
	- Wholesale
- Estimated Size of Deal: How many expandable/capsule houses do you need?
	- 1,2,3-5,5-10,10-50,50-100,100+
- Proposed Senario 
	- Self-build
	- Garden Suite
	- Rental Income
	- Resort/Trailer Park
	- Affordable Development
	- Other
- Buying Intent Measure
	- No Business: 
	- Curious: "I would like to take some marketing materials home."
	- Interested: "I would like to share my contact information and be posted for future updates."
	- Serious: "I am willing to pay a $100 refundable deposit to ensure I receive priority service when the product is ready to deploy."

```
https://site-api.costonegroup.com/utility/contact
POST Content-Type: application/x-www-form-urlencoded

full_name - [v1] [required], [string(1-64)], full name of lead
phone_number - [v1] [required/optional], [digits(10-14)], either phone or email is required
email_address - [v1] [required/optional], [string(5-128)], either phone or email is required, email will verify mx record of domain, so email like "random@fakemail.com" wont pass the validation
inquiry_message - [v1] [required], [json], inquiry information in json format
{
  "product": [
    "Flex-E20",
    "Flex-A30"
  ],
  "quantity": "5-10",
  "scenario": [
    "Generic Home",
    "Rental Income"
  ],
  "interest_level": "Planning to Buy"
}
```

```
#Product Inquiry

Full Name:
Phone:
Email:

Product - Which model are you interested in?
<select multiple, text with pictures>
1. EP-40(840 sqft, 40ft Expanable Home)
2. EP-20(430 sqft, 20ft Expanable Home)
3. AP-402(840 sqft, 40ft Stacked Apple Cabin Home)
4. AP-40(370 sqft, 40ft Apple Cabin Home)
5. AP-30(270 sqft, 30ft Apple Cabin Home)
6. CP-40(410 sqft, 40ft Capsule Home)
7. CP-30(300 sqft, 30ft Capsule Home)

Quantity - How many expandable/capsule houses do you need?
<radio, text>: 1,2,3-5,5-10,10-50,50-100,100+

Scenario - What is the intended use for our products?
<select multiple, text>: Generic Home, Garden Suite, Rental Income, Vacation Cottage, Resort/Trailer Park, Affordable Housing Development, Other

Interest Level - Which option best describes your situation?
<radio, text>
Exploring: I’m gathering information and not ready to commit yet.
Interested: I’d like to receive updates on your products.
Planning: I’m planning to make a purchase in the next 6–12 months.
Ready to Buy: I’d like to place a deposit and receive priority service.
```

## Web Content

##