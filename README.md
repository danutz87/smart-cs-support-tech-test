# Customer Service Support Technician Tech Test
Test for CS Support candidates

## Requirements

Allowing for setting up the project, this tech test should take around 2 hours to complete.

1. Find the cause of issues in the application:
* There are three bugs to find
* You are not required to *fix* the bugs, your aim is to __identify__ what is causing them

2. Document your process:
  * What did you notice/find?
  * What are your recommendations?

## My approach
I used user stories to try and find the bugs and this is what I found:

1. From the home page, when creating a new company and save it, it was always loading the show page for the first company, although the new company was saved to the list! 
        Home page => Create New Company => Save => Showing the details for the first company on the list (but the new company was added to the list)

    Also noticed that whenever you went to the companies list and wanted to see details about a specific company, again you will always see the details of the first company!
        Home page => Companies List  => Show => Showing the first company's on the list details

    Started to check what was causing it, noticed the redirection after save was good, so went to the Companies-Controller to check the methods and on the show method, there was an extra line of code: "@company = Company.first" reassigning every company with the details of the first company!

    **My solution:** delete this line of code from the show method "@company = Company.first" 

2. Trying to add a new employee, you get an error "Surname is required" although you have input in the form's field!
        Home page => Companies List  => Show => Add Employee =>  Error: "Surname is required"
    
    Checked the Employee model and found validations checks for forename, middlename and surname but noticed the form for adding a new employee had just two fields for forename and surname! After deleting the validation check for the middlename, the error still persisted, so went to check the add employee form's settings in the "new.html.erb" file in the employees views folder and found:
      <%= form.label :surname %>
      <%= form.text_field :middlename, class: "form-control" %>

    **My solution:** replace the form.text_field ":middlename" with ":surname" and either remove the validation check for the middlename or add another field to the add employee form but making the middlename optional as not everybody has a middlename!

3. When trying to edit the employees, other than the first company's, you would get an "ActiveRecord::RecordNotFound (Couldn't find company with id = 9)" error!
        Home page => Companies List => Show =>  Edit => Error

   After listing all the routes on the CLI, I started comparing the route for the edit employee button in the "show.html.erb" file in the companies views folder and found that the route for editing an employee was instead of "edit_company_employee_path(@company, employee)" was "edit_company_employee_path(employee)".

    **My solution:** replacing the route in the edit employee button from "edit_company_employee_path(employee)" to "edit_company_employee_path(@company, employee)"
