Exercise 9: Scrape Data from a Website and Store It in a .CSV File
Objective

To develop a robot in UiPath Studio that opens a website, searches for a product, extracts the resulting product listing (Name and Price) using the Data Scraping / Extract Data feature, and stores the extracted data in a .CSV file.

Algorithm
Step 1: START
Step 2: Open the target website in a browser using a Use Application/Browser activity
Step 3: Type the search keyword into the website's search box
Step 4: Click the search icon/button to load the search results page
Step 5: Use the Extract Data wizard to extract the product table (Name, Price) into a DataTable variable
Step 6: Write the DataTable to a CSV file using the Write CSV activity
Step 7: STOP

Step-by-Step Process
Open UiPath Studio and create a new Blank Process.
Drag a Use Application/Browser activity onto the workflow. Set the browser type to Chrome and the target URL to the website you want to scrape (e.g., https://www.walmart.com/).
Inside its Do body, add:
A Type Into activity — indicate the website's search box and type the keyword (e.g., "earphones").
A Click activity — indicate and click the search icon/button to load the results page.
Once the search results page loads, add an Extract Data activity (Data Scraping wizard):
Indicate the product listing table on the page.
Select the columns to extract (e.g., Name and Price) and configure pagination/extraction limits if needed.
Store the result in a DataTable variable (e.g., ExtractDataTable).
Add a Write CSV activity below the Extract Data activity:
Set the DataTable input to ExtractDataTable.
Set CSV Action to Write and enable Add Headers.
Specify the output File Path (e.g., "new-file.csv").
Save and run the workflow.
Open the generated CSV file to verify it contains the scraped product data with the Name and Price columns correctly populated.
Exercise 10: Fill a Webform Using Data Extracted from an Excel Sheet
Objective

To develop a robot in UiPath Studio that reads contact details (First Name, Last Name, Company Name, Role, Address, Email, Phone Number) from an Excel sheet and uses that data to automatically fill and submit an online webform (e.g., a Google Form).

Algorithm
Step 1: START
Step 2: Open the Excel file and read the data into a DataTable variable
Step 3: Open the target webform in a browser
Step 4: For each field on the form, type the corresponding value from the DataTable
Step 5: Click Submit to submit the form
Step 6: STOP

Step-by-Step Process
Open UiPath Studio and create a new Blank Process.
Drag an Excel Process Scope activity onto the workflow, and inside it add a Use Excel File activity, specifying the workbook path (e.g., "Excel_Sheet_Form_Filling.xlsx").
Inside the Use Excel File's Do body, add a Read Range activity to read the relevant sheet (e.g., "Extract_Data_From_Website") into a DataTable variable, dt.
(Optional, for testing) Add a Message Box activity to display dt.Rows.Count.ToString and confirm the data was read correctly.
Add a Use Application/Browser activity targeting the webform's URL (e.g., a Google Form) using Chrome.
Inside its Do body, add a series of Type Into activities — one per form field — indicating each field and typing the corresponding cell value from the DataTable:
First Name field ← dt.Rows(0)(1).ToString
Last Name field ← dt.Rows(1)(1).ToString
Company Name field ← dt.Rows(2)(1).ToString
Role in Company field ← dt.Rows(3)(1).ToString
Address field ← dt.Rows(4)(1).ToString
Email field ← dt.Rows(5)(1).ToString
Phone Number field ← dt.Rows(6)(1).ToString
Add a Click activity to click the Submit button on the form.
Save and run the workflow.
Check the form's confirmation page (or the linked response sheet) to verify all the details were filled in and submitted correctly.
Exercise 11: Read a Scanned Invoice Image Using OCR and Store the Extracted Data in a .CSV File
Objective

To develop a robot in UiPath Studio that opens a scanned invoice image, uses OCR (Optical Character Recognition) to read specific fields from it (Grand Total, Subtotal, Tax, and Customer Name), and stores the extracted data in a .CSV file.

Algorithm
Step 1: START
Step 2: Open the invoice image file
Step 3: Build an empty DataTable with two columns (Field, Value)
Step 4: Use OCR to read the Grand Total region of the image → Add row {"Grand Total", value}
Step 5: Use OCR to read the Subtotal region → Add row {"Subtotal", value}
Step 6: Use OCR to read the Tax region → Add row {"Tax", value}
Step 7: Use OCR to read the Customer Name region → Add row {"Name", value}
Step 8: Write the DataTable to a CSV file
Step 9: STOP

Step-by-Step Process
Open UiPath Studio and create a new Blank Process.
Declare two variables: dt (DataTable) and data (String).
(Optional) Add a Hotkey Trigger (Win + R) with Scheduling mode set to Sequential, if you want the automation to start via a shortcut.
Add a Use Application "Run" activity to open the invoice image:
Type Into the "Open" field — the full path to the invoice image (e.g., "D:\...\invoice.png").
Click OK — this opens the image in the Photos app.
Add a Build Data Table activity to create dt with two columns (e.g., Column1 and Column2, for Field and Value).
Add a Get OCR Text activity using the Tesseract OCR engine, targeting the invoice image window. Set the clipping region over the Grand Total amount on the invoice, and store the recognized text in the data variable.
Add an Add Data Row activity to insert {"Grand Total", data} into dt.
Repeat steps 6–7 for each remaining field:
Clip the Subtotal region → OCR into data → Add Data Row {"Subtotal", data}
Clip the Tax region → OCR into data → Add Data Row {"Tax", data}
Clip the customer Name region (Bill To) → OCR into data → Add Data Row {"Name", data}
Add a Write CSV activity: set the DataTable input to dt, CSV Action to Write, enable Add Headers, and specify the output File Path (e.g., "report1.csv").
Save and run the workflow.
Open the generated CSV file to verify it contains rows for Grand Total, Subtotal, Tax, and Name, with values correctly read from the invoice image.
Exercise 12: Read a True PDF File and Fill a Webform
Objective

To develop a robot in UiPath Studio that reads text from a true PDF (a digitally generated, non-scanned file), extracts a person's Name, Email, and Address from it, and uses that data to automatically fill and submit an online webform.

Algorithm
Step 1: START
Step 2: Extract the text content from the PDF file → store in variable output
Step 3: Split output by spaces into a string array elements (Name, Email, Address)
Step 4: Open the target webform in a browser
Step 5: Type elements(0) into the Name field
Step 6: Type elements(1) into the Email field
Step 7: Type elements(2) into the Address field
Step 8: Click Submit to submit the form
Step 9: STOP

Step-by-Step Process
Open UiPath Studio and create a new Blank Process.
Declare two variables: output (String) and elements (String array).
Add an Extract PDF Text activity: set the PDF File to the target file (e.g., "sample.pdf"), enable Apply OCR using the UiPath Document OCR engine (this keeps the workflow reliable for both true and scanned PDFs), and store the extracted text in output.
(Optional, for testing) Add a Message Box activity to display output and confirm the text was read correctly.
Add an Assign activity: elements = output.Split(" ") — this splits the extracted text into individual pieces (Name, Email, Address), assuming they appear space-separated in the PDF.
(Optional, for testing) Add a Message Box activity to display: "Name: " + elements(0) + Environment.NewLine + "Email: " + elements(1) + Environment.NewLine + "Address: " + elements(2) to verify the split values before filling the form.
Add a Use Application/Browser activity targeting the webform's URL (e.g., a Google Form titled "Personal Details") using your preferred browser.
Inside its Do body, add three Type Into activities:
Name field ← elements(0)
Email field ← elements(1)
Address field ← elements(2)
Add a Click activity to click the Submit button on the form.
Save and run the workflow.
Check the form's confirmation page (or the linked response sheet) to verify the Name, Email, and Address were filled in and submitted correctly.
Exercise 13: Read a Word File and Create a List of Unique Words in an Excel Sheet
Objective

To develop a robot in UiPath Studio that reads the content of a Word document, splits it into individual words, identifies the words that occur only once in the text (non-repeating words), and writes that list into an Excel/CSV file.

Algorithm
Step 1: START
Step 2: Read the full text of the Word document
Step 3: Remove punctuation (commas, full stops) from the text
Step 4: Split the text into an array of words using space as the delimiter
Step 5: Build an empty DataTable with one column
Step 6: For each word in the array:
            Count how many times it appears in the whole array
            If it appears more than once → skip it
            Else → add it to the DataTable
Step 7: Write the DataTable to an Excel/CSV file
Step 8: STOP

Step-by-Step Process
Open UiPath Studio and create a new Blank Process.
Declare three variables: text (String), arr (String array), and dt (DataTable).
Add a Word Application Scope activity: set the File Path to the target Word file (e.g., "input.docx").
Inside its Do body, add a Read Text activity to read the entire document content into the text variable.
Add an Assign activity to clean the text of punctuation: text = text.Replace(",", " ").Replace(".", " ")
Add another Assign activity to split the text into individual words: arr = text.Split(" ")
Add a Build Data Table activity to create dt with one column (e.g., Column1) to hold the resulting words.
Add a For Each activity to iterate over each word (oword) in arr:
Declare a counter (Int32) variable inside the loop body, and set it to 0 at the start of each iteration.
Add a nested For Each activity to iterate over every word (iword) in arr again:
Add an If activity with condition oword = iword; in the Then branch, increment counter by 1.
After the nested loop, add an If activity with condition counter > 1:
Then: do nothing (the word repeats elsewhere, so it's skipped).
Else: add an Add Data Row activity to insert {oword} into dt (the word occurs only once in the text).
After the outer loop, add a Write CSV (or Write Range, for an Excel sheet) activity to write dt to the output file (e.g., "output.csv"), with Add Headers enabled.
Save and run the workflow.
Open the output file to verify it lists only the words that appear exactly once in the Word document.
Exercise 14: Create a Queue in Orchestrator and Store the Subject of Emails in a .CSV File
Objective

To develop a robot in UiPath Studio that reads unread emails from a mailbox (e.g., Outlook 365), adds each email's subject as an item to an Orchestrator queue, then retrieves all items back from that queue and stores their subjects in a .CSV file.

Algorithm
Step 1: START
Step 2: Build an empty DataTable with one column (Subject)
Step 3: Connect to the mailbox, and for each unread email in the Inbox:
            Get the email's Subject
            Add the Subject as a new item to an Orchestrator queue
Step 4: Retrieve all items from the Orchestrator queue into a collection
Step 5: For each queue item retrieved, extract its Subject and add it as a row to the DataTable
Step 6: Write the DataTable to a CSV file
Step 7: STOP

Step-by-Step Process
Open UiPath Studio and create a new Blank Process.
Declare two variables: subjectdata (IEnumerable of QueueItem) and dt (DataTable).
Add a Build Data Table activity to create dt with one column, Subject.
Add a Use Outlook 365 (Exchange Application Scope) activity and sign in with the mailbox account.
Inside its Do body, add a For Each Email activity: set the folder to Inbox, enable Unread Only, and set a limit on the number of emails to process (e.g., 10).
Inside the For Each Email body:
Add an Assign activity: subject = CurrentMail.Subject
Add an Add Queue Item activity: set the Queue Name (e.g., "myqueue") and Folder Path (your Orchestrator workspace/folder), and pass the Item Information as {"Subject": subject}.
In Orchestrator, make sure the queue (e.g., "myqueue") already exists in the corresponding folder before running — create it manually via Orchestrator → Queues → Add Queue if needed.
After the email loop, add a Get Queue Items activity: set the Queue Name and Folder Path to match, select the relevant Queue Item States (New, InProgress, Failed, Successful, etc.), and store the result in subjectdata.
Add a For Each activity to iterate over subjectdata (as QueueItem):
Add an Add Data Row activity to insert item.SpecificContent("Subject") into dt.
Add a Write CSV activity after the loop: set the DataTable to dt, CSV Action to Write, enable Add Headers, and specify the File Path (e.g., "Untitled.csv").
Save and run the workflow.
Check the Orchestrator queue to confirm the items were added, and open the CSV file to verify it contains the subjects of the processed emails.
Exercise 15: Save Attachments from Unread Emails with 'Resume' in the Subject Line
Objective

To develop a robot in UiPath Studio that scans the unread emails in an Outlook inbox, filters those whose subject line contains the word "Resume" and that have attachments, and saves the attachments from those emails to a local folder.

Algorithm
Step 1: START
Step 2: Connect to the mailbox (e.g., Outlook 365)
Step 3: For each unread email in the Inbox that has attachments:
            If the email's Subject contains "Resume":
                Save all attachments of that email to a local folder
Step 4: STOP

Step-by-Step Process
Open UiPath Studio and create a new Blank Process.
Add a Use Outlook 365 (Exchange Application Scope) activity and sign in with the mailbox account.
Inside its Do body, add a For Each Email activity:
Set the folder to Inbox.
Enable Unread Only.
Enable With Attachments Only.
Set a limit on the number of emails to process (e.g., 10).
Add a Mail Filter: Criteria = Subject, Operator = Contains, Value = "Resume" (optionally restrict to a recent time window, e.g., the last 7 days, using the Date Filter option).
Inside the For Each Email body, add a Save Email Attachments activity:
Message: CurrentMail
Folder Path: the local folder where attachments should be saved (e.g., "C:\...\Attachments")
Overwrite Existing: False (so existing files with the same name aren't replaced)
Save and run the workflow.
Check the specified folder to confirm that attachments from unread emails with "Resume" in the subject have been saved correctly.
Exercise 16: Build a Data Table, Fill It with Data, and Check for Mismatching Columns Using Try Catch
Objective

To develop a robot in UiPath Studio that builds a DataTable with a strongly-typed column, fills it with user-provided data, and uses a Try Catch block to catch and handle any data-type/column mismatch errors that occur while adding the data.

Algorithm
Step 1: START
Step 2: Build an empty DataTable (dt) with one column (Column1), typed as Integer
Step 3: Try:
            Prompt the user to enter a value for age
            Add the entered value as a new row in dt
            If successful, write dt to a CSV file and display the entered age
        Catch (Exception):
            Display a message indicating the entered value does not match the column's data type
Step 4: STOP

Step-by-Step Process
Open UiPath Studio and create a new Blank Process.
Declare a variable: dt (DataTable).
Add a Build Data Table activity to create dt with one column, Column1, set to the Int32 data type.
Add a Try Catch activity.
Inside the Try block:
Add an Input Dialog activity with label "Enter age in wrong format" to take user input into a String variable, age (declared as a Try-Catch-scoped variable).
Add an Add Data Row activity: pass {age.ToString} as the ArrayRow into dt. Since Column1 expects an Integer, entering a non-numeric value here triggers a data-type mismatch exception.
Add a Write CSV activity: set the DataTable to dt, CSV Action to Write, enable Add Headers, and specify the File Path (e.g., "data.csv").
Add a Message Box activity to display "Age : " + age (this line only runs if no exception occurred).
In the Catch section, add a Catch block for System.Exception:
Add a Message Box activity to display "Age is not an Integer" — this runs whenever the entered value doesn't match the column's expected data type.
Save and run the workflow.
Test with a valid integer (e.g., 12) to confirm it's added to the DataTable and written to the CSV successfully; then test with a non-numeric value (e.g., "abc") to confirm the Catch block correctly reports the mismatch instead of crashing the robot.
Exercise 17: Generate a Monthly Expenditure Report by Extracting Data from a User Email Account (Using Re-Framework)
Objective

To develop a robot in UiPath Studio that scans a mailbox for unread emails with "Expenditure" in the subject, downloads their Excel attachments, reads the expense data from each file, aggregates it into a single report along with the total expenditure, and saves the result to a CSV file. In a full Re-Framework project, the email/attachment retrieval sits in the Get Transaction Data state and the read-and-aggregate logic sits in the Process state; the steps below show the core logic as a single sequence for lab purposes.

Algorithm
Step 1: START
Step 2: Build an empty DataTable (dtt) with two columns: Name, Amount
Step 3: Connect to the mailbox, and for each unread email with "Expenditure" in the subject:
            Save its Excel (.xls*) attachments to a local Attachments folder
Step 4: Get the list of all downloaded Excel files
Step 5: For each Excel file:
            Read its data (Sheet1) into a DataTable
            For each row in that DataTable:
                Add the row to the master DataTable (dtt)
                Add the row's Amount to a running total
Step 6: Add a final row {"Total Expenditure", total} to dtt
Step 7: Write dtt to a CSV file
Step 8: STOP

Step-by-Step Process
Open UiPath Studio and create a new process (ideally based on the Re-Framework template, where this logic lives inside the Get Transaction Data / Process states — a straightforward sequence also works for a lab demo).
Declare variables: ExcelFile (String array), total (Int32, default 0), dtt (DataTable).
Add a Build Data Table activity to create dtt with two columns: Name (String) and Amount (Int32).
Add a Use Outlook 365 (Exchange Application Scope) activity and sign in with the mailbox account.
Inside its Do body, add a For Each Email activity:
Folder: Inbox
Enable Unread Only and With Attachments Only
Set a limit (e.g., 10 emails)
Add a Mail Filter: Criteria = Subject, Operator = Contains, Value = "Expenditure" (optionally restricted to the last 7 days).
Inside the For Each Email body, add a Save Email Attachments activity:
Message: CurrentMail
Filter: "*.xls*" (Excel attachments only)
Folder Path: a local folder (e.g., "Attachments")
Overwrite Existing: False
After the email loop, add an Assign activity: ExcelFile = Directory.GetFiles("Attachments")
Add a For Each activity to iterate over ExcelFile (each currentText = a file path):
Add a Use Excel File activity: set the Workbook Path to currentText.
Inside its Do body:
Add a Read Range activity to read Sheet1 into a DataTable variable, data.
Add a For Each Row activity to iterate over data:
Add an Add Data Row activity: pass CurrentRow.ItemArray into dtt.
Add an Assign activity: total = total + Integer.Parse(CurrentRow.Item(1).ToString) to accumulate the Amount column into the running total.
After the outer loop, add an Add Data Row activity to insert {"Total Expenditure", total.ToString} into dtt as a summary row.
Add a Write CSV activity: set the DataTable to dtt, CSV Action to Write, enable Add Headers, and specify the File Path (e.g., "Result.csv").
Save and run the workflow.
Open the resulting report to verify it lists each expense entry (Name, Amount) from every downloaded attachment, along with a final Total Expenditure row showing the combined amount.
