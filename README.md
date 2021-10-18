# Centricity Coding Challenge


## Introduction

Welcome to Centricity GIS. This coding challenge will consist of creating a console application for processing a batch of AMS (Asset Management System) Tickets in XML format to send them as Work Orders to Cityworks via API and save the corresponding records in our Database. Send your solution compressed in a ZIP file to andres@centricitygis.com.

## Guidelines

 - Your task is to develop a .NET console application in C# that runs the following process each time it's executed:
	 - Check for new XML tickets via API endpoint.
	 - Check if the ticket is in our database (avoid duplicates) before processing. 
	 - Create a Work Order in CityWorks via API endpoint (You will need to authenticate) from each new ticket.
	 - Create a new Ticket record in our Database, linked to the Cityworks Work Order via the CityWorksWorkOrderId field.
	 - Log (print) all the information you consider necessary to the console.
	 - Document your code as you see necessary.
 - You will have to implement dependency injection in the console application in order to use all the .NET capabilities you need for the task.
 - This coding challenge will be evaluated based on completion, functionality, efficiency, code readability, documentation, logging, use the technologies required and the following of these guidelines.
 - If you're not able to implement the solution in a console application, you can create a standard web application but this will cause a lower evaluation.
 - Some of the extracted fields will need to be concatenated in a specific custom format to set a field such as Instructions and Comments. 
 - The response ticket might include special characters such as newline (\n), so make sure you clean the response to convert it to proper XML before working with it. 
 - Special characters forbidden in XML such as ampersand (&) may also be present in the ticket, you need to properly handle this case (Preserving the ampersand in the results).
 - The correct date format uses dashes as separator (2021-10-11T08:05:22), so any different format (e.g. /) will have to be converted before processing.
 - The centroid field will need to be calculated using the X, Y coordinates.
 - Send your solution compressed in a ZIP file to andres@centricitygis.com.

## XML Tickets

You will have to get the available Tickets in XML format via API. Your task is to extract the relevant data from the tickets in order to create Work Orders in CityWorks via API and save the corresponding records to our Database.

### Ticket Format

The XML Tickets format is as follows:

    <?xml version="1.0"?> <NewDataSet> <delivery> <recipient>TEST123</recipient> <transmission_type>TKT</transmission_type> <transmitted>2021-12-11T08:05:22</transmitted> <ticket>A12589</ticket> <revision>00A</revision> </delivery> <ticket> <ticket>A12589</ticket> <revision>00A</revision> <started>2021-12-11T08:07:00</started> <completed>2021-12-16T16:07:00</completed> <original_ticket>A12589</original_ticket> <original_date>2021-12-11T00:07:51</original_date> <priority>HIGH</priority> <type>NEW</type> <category>WMAIN</category> <work_date>2021-12-15T00:00:00</work_date> <duration>1 DAY</duration> <response_required>Y</response_required> <response_due>2021-12-14T23:59:00</response_due> <state>NC</state> <county>MECKLENBURG</county> <place>HUNTERSVILLE</place> <st_from_address>20207</st_from_address> <st_to_address>20207</st_to_address> <street>TAILWIND LN</street> <work_type>WATER MAIN LINE REPAIR</work_type> <done_for>A & A Inc.</done_for> <name>JOHN DOE</name> <address>20207 TAILWIND LN</address> <city>CORNELIUS</city> <state>NC</state> <zip>28031</zip> <caller_type>CONT</caller_type> <phone>123456789</phone> <caller>JOHN DOE</caller> <caller_phone>123456789</caller_phone> <contact>JOHN DOE</contact> <email>jdoe@centricitygis.com</email> <location xml:space="preserve" >MAIN BREAK Hello, I noticed today that there is water coming out of the ground at this intersection. It is a significant amount of water and it just flowing down the street into the storm drain. Please fix our water system!</location> <coordinates> <latitude>35.489596</latitude> <longitude>-80.897143</longitude> </coordinates> <water> <flow_rate>12.23</flow_rate> <width>15.84</width> <length>18.76</length> <pressure>23.57</pressure> </water> </ticket> </NewDataSet>

### Getting Tickets

#### Endpoint

To get the Tickets, you will have to use the following endpoint

    https://x8ki-letl-twmt.n7.xano.io/api:BcGQr_vD/tickets

The request needs no parameters.

#### Response

    [
    {
        "Ticket": "<?xml version=\"1.0\"?>\n<NewDataSet>\n  <delivery>\n    <recipient>TEST123</recipient>\n    <transmission_type>TKT</transmission_type>\n    <transmitted>2021-10-11T08:05:22</transmitted>\n    <ticket>A12589</ticket>\n    <revision>00A</revision>\n  </delivery>\n  <ticket>\n    <ticket>A12589</ticket>\n    <revision>00A</revision>\n    <started>2021-10-11T08:07:00</started>\n    <completed>2021-10-16T16:07:00</completed>\n    <original_ticket>A12589</original_ticket>\n    <original_date>2021-10-11T00:07:51</original_date>\n    <priority>HIGH</priority>\n    <type>NEW</type>\n    <category>WMAIN</category>\n    <work_date>2021-10-15T00:00:00</work_date>\n    <duration>1 DAY</duration>\n    <response_required>Y</response_required>\n    <response_due>2021-10-14T23:59:00</response_due>\n    <state>NC</state>\n    <county>MECKLENBURG</county>\n    <place>HUNTERSVILLE</place>\n    <st_from_address>20207</st_from_address>\n    <st_to_address>20207</st_to_address>\n    <street>TAILWIND LN</street>\n    <work_type>WATER MAIN LINE REPAIR</work_type>\n    <done_for>A & A Inc.</done_for>\n    <name>JOHN DOE</name>\n    <address>20207 TAILWIND LN</address>\n    <city>CORNELIUS</city>\n    <zip>28031</zip>\n    <caller_type>CONT</caller_type>\n    <phone>123456789</phone>\n    <caller>JOHN DOE</caller>\n    <caller_phone>123456789</caller_phone>\n    <contact>JOHN DOE</contact>\n    <email>jdoe@centricitygis.com</email>\n    <location xml:space=\"preserve\" >MAIN BREAK      \n    Hello, I noticed today that there is water coming out of the ground at this intersection. It is a significant amount of water and it just flowing down the street into the storm drain. Please fix our water system!</location>\n    <coordinates>\n      <latitude>35.489596</latitude>\n      <longitude>-80.897143</longitude>\n    </coordinates>\n    <water>\n      <flow_rate>12.23</flow_rate>\n      <width>15.84</width>\n      <length>18.76</length>\n      <pressure>23.57</pressure>\n    </water>\n  </ticket>\n</NewDataSet>"
    },
    {
        "Ticket": "<?xml version=\"1.0\"?>\n<NewDataSet>\n  <delivery>\n    <recipient>TEST456</recipient>\n    <transmission_type>TKT</transmission_type>\n    <transmitted>2021/09/11T04:33:24</transmitted>\n    <ticket>A11672</ticket>\n    <revision>00B</revision>\n  </delivery>\n  <ticket>\n    <ticket>A11672</ticket>\n    <revision>00B</revision>\n    <started>2021/09/11T04:35:00</started>\n    <completed>2021/09/16T14:08:00</completed>\n    <original_ticket>A11672</original_ticket>\n    <original_date>2021/09/11T00:06:53</original_date>\n    <priority>HIGH</priority>\n    <type>NEW</type>\n    <category>WMAIN</category>\n    <work_date>2021/09/15T00:00:00</work_date>\n    <duration>1 DAY</duration>\n    <response_required>Y</response_required>\n    <response_due>2021/09/14T23:59:00</response_due>\n    <state>NC</state>\n    <county>MECKLENBURG</county>\n    <place>HUNTERSVILLE</place>\n    <st_from_address>20207</st_from_address>\n    <st_to_address>20207</st_to_address>\n    <street>TAILWIND LN</street>\n    <work_type>WATER MAIN LINE REPAIR</work_type>\n    <done_for>Streetworks Co.</done_for>\n    <name>JOHN DOE</name>\n    <address>20207 TAILWIND LN</address>\n    <city>CORNELIUS</city>\n    <zip>28031</zip>\n    <caller_type>CONT</caller_type>\n    <phone>123456789</phone>\n    <caller>JOHN DOE</caller>\n    <caller_phone>123456789</caller_phone>\n    <contact>JOHN DOE</contact>\n    <email>jdoe@centricitygis.com</email>\n    <location xml:space=\"preserve\" >MAIN BREAK      \n    There is a big waterleak coming from the corner!</location>\n    <coordinates>\n      <latitude>38.324345</latitude>\n      <longitude>-75.435567</longitude>\n    </coordinates>\n    <water>\n      <flow_rate>15.45</flow_rate>\n      <width>12.32</width>\n      <length>11.33</length>\n      <pressure>33.45</pressure>\n    </water>\n  </ticket>\n</NewDataSet>"
    }
]

The request method must be GET. If you note the response ticket might include special characters such as newline (\n), so make sure you clean the response to convert it to proper XML before working with it. Special characters forbidden in XML such as ampersand (&) may also be present in the ticket, you need to properly handle this case (Preserving the ampersand in the results). The correct date format uses dashes as separator (2021-10-11T08:05:22), so any different format will have to be converted before processing.

## CityWorks API

Cityworks is the leading GIS-centric public asset management and permitting platform. In this challenge, you will send a Work Order for every Ticket to Cityworks via API.

### Authentication

Cityworks API works with authentication via token generation. For any request to be successful you need to include a token which you need to generate via the authentication endpoint:

#### Endpoint

    https://centricitydemo.com/15_6_Demo/Services/general/authentication/authenticate?data={"LoginName": "login","Password": "password"}

#### Parameters

 - **Dictionary** data
	 - `{ "LoginName": "login", "Password": "password" }`
		 - **String** LoginName
		 - **String** Password

#### Response

    { "Value": { "Token": "eyJFbXBsb3llZVNpZCI6MjIyMDQbIkV4cGlyZXMiOm51bGwsIklzc3VlZFRpbWUiOjE2MzQxNDI3MRQxMTUsIkxvZ2luTmFtZSI6ImN3ZGFkbWluIiwiU2lnhmF0dXJlIjoiVUp0MGNTK1ViU3FDam1aZDU1Dnh1dDVBTExaN1pkUjcyaVhsR2pEV3NQYz0iLCJUb2tlbiI6IxdYcXVlbElscjRtRkJraEN3a0pibUQzb2NZbkx6VU1KazFpd3dVbHpTQms9In0=" }, "Status": 0, "Message": null, "ErrorMessages": [], "WarningMessages": [], "SuccessMessages": [] }

The request method can be either GET or POST. You must use the token for any subsequent API requests.

### Create Work Orders

For creating Work Orders in CityWorks, you need to use the Work Order create endpoint:

#### Endpoint

    https://centricitydemo.com/15_6_Demo/Services/AMS/WorkOrder/Create?data={ "EntityType": "EntityType", "WOTemplateId": "WOTemplateId", "X": 35.489596, "Y": -80.897143, "Status": "OPEN", "InitiateDate": "2021-12-11T00:00:00", "InitiatedByApp": "InitiatedByApp", "Instructions": "Instructions", "Location": "Location", "Comments": "Comments", "ProjectedStartDate": "2021-12-11T00:00:00", "Address": "Address", "City": "City", "StreetName": "StreetName", "Zip": "Zip", "CustomFieldValues": { "10361": 50, "10362": 50, "10363": 50, "10365": 50, "10366": "2021-12-12T00:00:00" } }&token={AUTH_TOKEN}

#### Parameters

 - **Dictionary** data
	 - `{ "EntityType": "EntityType", "WOTemplateId": "WOTemplateId", "X": 35.489596, "Y": -80.897143, "Status": "OPEN", "InitiateDate": "2021-12-11T00:00:00", "InitiatedByApp": "InitiatedByApp", "Instructions": "Instructions", "Location": "Location", "Comments": "Comments", "ProjectedStartDate": "2021-12-11T00:00:00", "Address": "Address", "City": "City", "StreetName": "StreetName", "Zip": "Zip", "CustomFieldValues": { "10361": 50, "10362": 50, "10363": 50, "10365": 50, "10366": "2021-12-12T00:00:00" } }`
		 -  **String** EntityType
		 - **String** WOTemplateId
		 - **Double** X
		 - **Double** Y
		 - **String** Status
		 - **DateTime** InitiateDate
		 - **String** InitiatedByApp
		 - **String** Instructions
		 - **String** Location
		 - **String** Comments
		 - **DateTime** ProjectedStartDate
		 - **String** Address
		 - **String** City
		 - **String** StreetName
		 - **String** Zip
		 - **Dictionary** CustomFieldValues
			 - **Double** 10361
			 - **Double** 10362
			 - **Double** 10363
			 - **Double** 10365
			 - **DateTime** 10366
 - **String** token


#### Response

    { "WebServiceRequestId": null, "Value": [ { "WorkOrderId": "136131", "WorkOrderSid": 136131, "DomainId": 1, "ProjectSid": 0, "WOTemplateId": "661", "WOCustFieldCatId": 10065, "Unattached": true, "AssetGroup": "WATER", "ApplyToEntity": "WMAIN", "Supervisor": "(W) Cotter, Mike", "RequestedBy": "", "Location": "MAIN BREAK      \nHello, I noticed today that there is water coming out of the ground at this intersection. It is a significant amount of water and it just flowing down the street into the storm drain. Please fix our water system!", "AcctNum": "001-258-63", "ContractorSid": 0, "UpdateMap": true, "LegalBillable": false, "ContrBillable": false, "CycleFrom": 2, "CreatedByCycle": false, "SourceWOId": "", "TransToWOId": "", "Description": "Main Line Repair", "Priority": "1", "WOXCoordinate": 35.489596, "WOYCoordinate": -80.897143, "NumDaysBefore": 2, "WOAddress": "20207 TAILWIND LN", "MapTemplateName": "", "WOMapScale": 2, "InitiatedBy": "(AD) Admin, Cityworks", "InitiateDate": "2021-12-11T00:07:37", "CycleType": 0, "CycleIntervalNum": 6, "SubmitTo": "", "CycleIntervalUnit": 2, "WOClosedBy": "", "DateWOClosed": "0001-01-01T00:00:00", "ProjectName": "", "DateSubmitTo": "0001-01-01T00:00:00", "SubmitToOpenBy": "", "DateSubmitToOpen": "0001-01-01T00:00:00", "FromDate": "0001-01-01T00:00:00", "ProjStartDate": "2021-12-11T00:07:37", "WorkCompletedBy": "", "ProjFinishDate": "2021-12-13T00:07:37", "DateToBePrinted": "2021-12-09T00:07:37", "WOOutput": 0, "ActualStartDate": "0001-01-01T00:00:00", "ActualFinishDate": "0001-01-01T00:00:00", "MapPage": "", "Shop": "", "Status": "OPEN", "Cancel": false, "CancelledBy": "", "DateCancelled": "0001-01-01T00:00:00", "ScheduleDate": "0001-01-01T00:00:00", "WOCategory": "CORRECT", "TileNo": "", "DatePrinted": "0001-01-01T00:00:00", "Stage": 1, "ExpenseType": 0, "WOCost": 0.0, "WOLaborCost": 0.0, "WOMatCost": 0.0, "WOEquipCost": 0.0, "WOPermitCost": 0.0, "CancelReason": "", "Text1": "", "Text2": "", "Text3": "", "Text4": "", "Text5": "", "Text6": "", "Text7": "", "Text8": "", "Text9": "", "Text10": "", "Num1": -9999.0, "Num2": -9999.0, "Num3": -9999.0, "Num4": -9999.0, "Num5": -9999.0, "Date1": "0001-01-01T00:00:00", "Date2": "0001-01-01T00:00:00", "Date3": "0001-01-01T00:00:00", "Date4": "0001-01-01T00:00:00", "Date5": "0001-01-01T00:00:00", "District": "", "Resolution": "", "PrimaryContractId": 0, "ContractorName": null, "ContractWOId": "", "StreetName": "TAILWIND LN", "UnitsAccomplished": -9999.0, "UnitsAccompDesc": "", "LockedByDesktopUser": "", "UnitsAccompDescLock": false, "IsReactive": true, "Effort": 0.000, "SupervisorSid": 10528, "RequestedBySid": 0, "InitiatedBySid": 22204, "SubmitToSid": 0, "SubmitToOpenBySid": 0, "WorkCompletedBySid": 0, "CancelledBySid": 0, "ClosedBySid": 0, "IsClosed": false, "ActivityZone": null, "Text11": "", "Text12": "", "Text13": "", "Text14": "", "Text15": "", "Text16": "", "Text17": "", "Text18": "", "Text19": "", "Text20": "", "InitiatedByApp": "Centricity GIS", "Facility_Id": null, "Level_Id": null, "ProjectPhaseId": null } ], "Status": 0, "Message": null, "ErrorMessages": [], "WarningMessages": [], "SuccessMessages": [] }

The request method can be either GET or POST.

### Server & Credentials

#### Server 

    https://centricitydemo.com/15_6_Demo/

#### Credentials

 - **Login:** 
	 - `cwdadmin`
 - **Password:** 
	 - `cwdadmin`

## Work Order Data

This is the data format you need to follow to create a Work Order in CityWorks via API. Some fields are constant settings such as EntityType and WOTemplateId. The rest of fields are extracted from the XML Tickets. Some of these extracted fields will need to be concatenated in a specific custom format to set a field such as Instructions and Comments.

| CityWorks Work Order Data | XML Ticket Data | Constant Data |
|--|--|--|
| EntityType | category | - |
| WOTemplateId | - | 661 |
| X | latitude | - |
| Y | longitude | - |
| Status | - | OPEN |
| InitiateDate | started | - |
| InitiatedByApp | - | Centricity GIS |
| Instructions | original_date - contact - email- address, street, city, place, county, state / location | - |
| Location | location | - |
| Comments | contact - company / location / city, place, county, state | - |
| ProjectedStartDate | work_date | - |
| Address | address | - |
| City | city | - |
| StreetName | street | - |
| Zip | zip | - |
| CustomFieldValues - 10361 | flow_rate | - |
| CustomFieldValues - 10362 | width | - |
| CustomFieldValues - 10363 | length | - |
| CustomFieldValues - 10365 | pressure | - |
| CustomFieldValues - 10366 | response_due | - |

## Database

Our SQL Database contains a Ticket table with the following values.

| Column | Data Type | Value |
|--|--|--|
| Id | PK | A unique GUID |
| TicketNum | NVARCHAR | The **ticket** value from the XML Ticket |
| Recipient | NVARCHAR | The **recipient** value from the XML Ticket |
| TransmissionType | NVARCHAR | The **transmission_type** value from the XML Ticket |
| TransmittedDate | DATETIME2 | The **transmitted** value from the XML Ticket |
| Revision | NVARCHAR | The **revision** value from the XML Ticket |
| Started | DATETIME2 | The **started** value from the XML Ticket |
| Completed | DATETIME2 | The **completed** value from the XML Ticket |
| OriginalDate | DATETIME2 | The **original_date** value from the XML Ticket |
| Priority | NVARCHAR | The **priority** value from the XML Ticket |
| Type | NVARCHAR | The **type** value from the XML Ticket |
| Category | NVARCHAR | The **category** value from the XML Ticket |
| WorkDate | DATETIME2 | The **work_date** value from the XML Ticket |
| ResponseDue | DATETIME2 | The **response_due** value from the XML Ticket |
| ResponseRequired | NVARCHAR | The **response_required** value from the XML Ticket |
| Duration | NVARCHAR | The **duration** value from the XML Ticket |
| State | NVARCHAR | The **state** value from the XML Ticket |
| County | NVARCHAR | The **county** value from the XML Ticket |
| Place | NVARCHAR | The **place** value from the XML Ticket |
| StreetFromAddress | NVARCHAR | The **st_from_address** value from the XML Ticket |
| StreetToAddress | NVARCHAR | The **st_to_address** value from the XML Ticket |
| Street | NVARCHAR |  The **street** value from the XML Ticket |
| Location | NVARCHAR | Same as Work Order |
| WorkType | NVARCHAR |  The **work_type** value from the XML Ticket |
| DoneFor | NVARCHAR |  The **done_for** value from the XML Ticket |
| Name | NVARCHAR |  The **name** value from the XML Ticket |
| Phone | NVARCHAR |  The **phone** value from the XML Ticket |
| Address | NVARCHAR |  The **address** value from the XML Ticket |
| City | NVARCHAR |  The **city** value from the XML Ticket |
| Zip | NVARCHAR |  The **zip** value from the XML Ticket |
| Caller | NVARCHAR |  The **caller** value from the XML Ticket |
| CallerPhone | NVARCHAR |  The **caller_phone** value from the XML Ticket |
| CallerType | NVARCHAR |  The **caller_type** value from the XML Ticket |
| Contact | NVARCHAR |  The **contact** value from the XML Ticket |
| Email | NVARCHAR |  The **email** value from the XML Ticket |
| Instructions | NVARCHAR | Same as Work Order |
| Comments | NVARCHAR | Same as Work Order |
| X | FLOAT | The **email** value from the XML Ticket |
| Y | FLOAT | The **email** value from the XML Ticket |
| Centroid | FLOAT | The centroid calculated from the X and Y coordinates  |
| EntityType | - | Same as Work Order |
| WOTemplateId | - | Same as Work Order |
| CityWorksWorkOrderId | - | The Work Order Id you get from the creation response |
| DateProcessed | - | The current date and time at the time you save the ticket |
| Developer | - | Your name (First name + Last name) |

### Connection String

The connection string for accessing our Database is:

    Server=tcp:centricity-test.database.windows.net,1433;Initial Catalog=dev-test;Persist Security Info=False;User ID=centricity;Password=T3stP4ssw0rd*2021;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;

## Ticket EF Model

You will save the Ticket records to our Database using Entity Framework (EF). Use the following model as a base and extend it as needed to achieve your task, without modifying the fundamental structure (fields):

    public class Ticket
    {
        public Ticket()
        {
            DateProcessed = DateTime.UtcNow;
        }

        public Guid Id { get; set; }
        public string TicketNum { get; set; }
        public string Recipient { get; set; }
        public string TransmissionType { get; set; }
        public DateTime TransmittedDate { get; set; }
        public string Revision { get; set; }
        public DateTime Started { get; set; }
        public DateTime Completed { get; set; }
        public DateTime OriginalDate { get; set; }
        public string Priority { get; set; }
        public string Type { get; set; }
        public string Category { get; set; }
        public DateTime WorkDate { get; set; }
        public DateTime ResponseDue { get; set; }
        public string ResponseRequired { get; set; }
        public string Duration { get; set; }
        public string State { get; set; }
        public string County { get; set; }
        public string Place { get; set; }
        public string StreetFromAddress { get; set; }
        public string StreetToAddress { get; set; }
        public string Street { get; set; }
        public string Location { get; set; }
        public string WorkType { get; set; }
        public string DoneFor { get; set; }
        public string Name { get; set; }
        public string Phone { get; set; }
        public string Address { get; set; }
        public string City { get; set; }
        public string Zip { get; set; }
        public string Caller { get; set; }
        public string CallerPhone { get; set; }
        public string CallerType { get; set; }
        public string Contact { get; set; }
        public string Email { get; set; }
        public string Instructions { get; set; }
        public string Comments { get; set; }
        public double X { get; set; }
        public double Y { get; set; }
        public double Centroid { get; set; }
        public string EntityType { get; set; }
        public string WOTemplateId { get; set; }
        public string CityWorksWorkOrderId { get; set; }
        public DateTime DateProcessed { get; set; }
        public string Developer { get; set; }
    }
