# Hello World

4/12 Steps

Get prescription.FillsAllowedQuantity
Get fills.fillNumber
Check if refillsRemaining (prescription.FillsAllowedQuantity - fills.fillNumber) = 0
	fills is a list
		Go through all fields in the list and get max value for fillNumber

Get prescription.expirationDate
Get sentTimestamp
	sentTimestamp does not exist in Prescription JSON, needs to be pulled from Event API json.
		This might change later, for now it is fine to build like this.
Get prescription.fills.dispensedItems.daysSupplyQuantity 
	Since dipensedItems is also a list, pick latest field value for this 
	Make sure latest value in dispensedItems is item.type = "Drug"
		In case of more than one Drug result, get first one in the list (already agreed with Business)
		Ignore all non-Drug results
	This extraction logic already exists in RxRequestProcessor, can take from there
	prescriptionRelativeId from Event json might work, map it to prescription.fills.relativeId (Does not match tho, after testing. Use above method instead in the meantime. Roberto to review first if worth investing time in this method)
Check if expirationDate <= sentTimestamp + daysSupplyQuantity
Check if sentTimestamp + daysSupplyQuantity + 60 <= expirationDate
	The number 60 is a constant
		Add this constant to config server
		
If any of the validations are true, then call another API (RxRequestProcessor, this call is not covered in this US. That API is not ready yet)
	If all three validations are false, end the process.
	Log the components and results of all three formulas

Add comments to US for clarification after finishing.

4/13 Steps
Added as comments to PR by Sunita:
	Remove repositories from pom.xml
	Remove plugin repositories from pom.xml
	Rename MainApplication class to RxRequestProcessorEventFilter

Requested by IM by Sunita but not added to PR: 
	Refactor and remove "rref" from folder structure
