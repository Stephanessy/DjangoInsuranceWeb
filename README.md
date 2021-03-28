Wed Application Design & Construction

### Development Environments

language: python 3.6.8, HTML

Web framework: Django 3.0.4

Database: MySQL Workbench 8.0 CE

IDE: PyCharm

---

### Django Logic

*This section introduces the logic of the project.*

(1) The first thing to do is creating a new django project.

(2) Then we need to connect the database to the project. This include creating authentication user for the schema, `$ python manage.py inspectdb > models.py` command, etc. Within the django project, `models.py` must contains all the entities that database schema contains.

(3) `settings.py` contains configuration information about database and apps created in the project. `urls.py` is the root URL conguration of the project.

(4) static directory is used to collect our project wide static assets.

(5) Inside `views.py`, we can define Python function or class that takes a Web request and returns a Web response. Also we add check and process for datas from web, ensuring data finally saved to database is true.

(6) templates directory contains all the templates the project needed. A
template contains the static parts of the desired HTML output as well as
some special syntax describing how dynamic content will be inserted.

![](images/2020-06-20-20-26-02.png)

*Figure 1: Django project logic*

Figure 1 displays the running logic of django project. Model and View is on the server side, Template is on the client side. View can be used to create, update or delete records of model. It can also provide data for display. Template is used to display data for users. Users can input data via template.

---

### Database

#### Relational Model

![](images/2020-06-20-20-29-46.png)

*Figure 2: Relational Model*

*Full resolution picture of relational model is provided in a separate file.*

Here is part of DDL code:

![](images/2020-06-20-20-32-09.png)

*Figure 3: DDL code (part)*

*Full code is provided in a separate sql file final.sql.*

---

### Features
*This section depicts CRUD, extra features and application procedure of our website.*

#### Register, Login and Logout

This section introduces the sign up, login and logout feature of the
project. Sign up view function will create a record in default User model with a primary key auto-created. During registering, make password is called so that password is encrypted using the PBKDF2 algorithm (Password-Based Key Derivation Function 2) with a SHA256 hash. When logging in, the pass- word typed will be encrypted and send to server. After logout, the session data for the current request is completely cleaned out.

![](images/2020-06-20-20-37-34.png)

*Figure 4: Login*

![](images/2020-06-20-20-38-00.png)

*Figure 5: Sign up*

#### Personal Center

This section introduces the update feature of the project. A user can
update personal information displayed in personal center. Insurance related information can also be modified, however, this operation is reserved for administrator.

![](images/2020-06-20-20-39-06.png)

*Figure 6: Personal Center*

![](images/2020-06-20-20-39-28.png)

*Figure 7: Update Info*

#### Purchase

This section introduces the insert feature of the project. After user purchase an insurance, necessary records will be inserted into different tables of the database. Take home insurance for example:

(1) Start page for home insurance:

![](images/2020-06-20-20-40-26.png)

*Figure 8: Start page*

(2) Fill insurance information:

![](images/2020-06-20-20-40-53.png)

*Figure 9: Home Insurance*

(3) Start date cannot be earlier than today:

![](images/2020-06-20-20-41-25.png)

*Figure 10: Start date restriction*

(4) End date chosen by option:

![](images/2020-06-20-20-41-53.png)

*Figure 11: End date restriction*

(5) Fill up insured home information:

![](images/2020-06-20-20-42-29.png)

*Figure 12: Insured Home*

(6) View order information and pay:

![](images/2020-06-20-20-42-35.png)

*Figure 13: Order Info*

(7) View invoice:

![](images/2020-06-20-20-43-02.png)

*Figure 14: Invoice*

(8) Make payment:

![](images/2020-06-20-20-43-07.png)

*Figure 15: Payment*

#### Insurance,Invoice & Payment Query

This section introduces the query feature of the project. There are two kinds of query in this project, insurance query and invoice query. Insurance query consists of home insurance query and auto insurance query. User can only query on his or her own data, including insurance information, house information, vehicle information, driver information, etc. Invoice query is used for displaying user's invoices. User can also use this entrance to pay installment invoices.

(1) Query Insurance information:

![](images/2020-06-20-20-44-33.png)

*Figure 16: Home Insurance Query*

![](images/2020-06-20-20-44-36.png)

*Figure 17: Home Insurance Info*

![](images/2020-06-20-20-44-46.png)

*Figure 18: Home Info*

(2) Query invoice and payment: this is designed for users to check all their invoice details and pay installment. If one choose to pay installments, there will be multiple invoice for one insurance.

![](images/2020-06-20-20-45-36.png)

*Figure 19: Home Invoice Query*

![](images/2020-06-20-20-46-22.png)

*Figure 20: Home Invoice Query Result*

#### Superuser Function

This section introduces the delete feature of the project. Delete feature is designed for administrator. An administrator can delete a certain insurance record and all related information.

(1) Input insurance ID to delete:

![](images/2020-06-20-20-46-56.png)

*Figure 21: Delete*

(2) Confirm delete:

![](images/2020-06-20-20-47-39.png)

*Figure 22: Delete Confirm*

(3) Click to delete and lift result message:

![](images/2020-06-20-20-47-46.png)

*Figure 23: Delete Result*

#### Extra Feature Summary

(1) Index

![](images/2020-06-20-20-49-24.png)

*Figure 24: Index*

For the index feature, we follow the thumb rule that we build index on
attributes that are frequently queried. In Django, it is easy to enable index feature, with the statement `db_index=Ture` above, it builds a B+ tree index on the attribute.

(2) Password encryption

![](images/2020-06-20-20-49-31.png)

*Figure 25: Encryption*

After create user, change password attribute by using `make_password()` to encrypt. Then use `auth.authenticate()` below to authenticate users to login.

![](images/2020-06-20-20-50-05.png)

*Figure 26: Authenticate*

(3) CSRF

![](images/2020-06-20-20-50-52.png)

*Figure 27: CSRF*

Add `{% csrf tocken %}` to get protection from CSRF (Cross Site Re- quest Forgery) attack.

(4) Status Update (automatic)

![](images/2020-06-20-20-50-57.png)

*Figure 28: Status Update*

Achieve a function for superuser to update insurance status and customer type, also, this update function is embedded in personal center (interface to view related information), every time a user access personal center, it will automatically update status of current user.

(5) SQL Injection

![](images/2020-06-20-20-54-00.png)

*Figure 29: SQL Injection*

In the aspect of SQL Injection, we prevent this by using Django ORM
function, it process input by only accepting value for corresponding attribute, thus protect from injection.

(6) jQuery

![](images/2020-06-20-20-54-19.png)

*Figure 30: jQuery*

When designing template for home insurance and auto insurance, we use jQuery to dynamically set constraint for certain input tag (start date) so that user cannot choose a date before today as the start date for home or auto insurance. We also use option to restrict end date such that user cannot choose a date earlier than start date. These restrictions for input is essential for the stable of database.

(7) Beautified UI

Figures below are UI sample we beautified by HTML, CSS, bootstrap
templates.

![](images/2020-06-20-20-56-19.png)

*Figure 31: UI sample -1*

![](images/2020-06-20-20-56-28.png)

*Figure 32: UI sample -2*

![](images/2020-06-20-20-56-33.png)

*Figure 33: UI sample -3*

## Learning Outcome

(1) About input constraint

When considering about input for a practical website, we have to think
about constraints, that doesn't mean we let invalid input trigger error in database, we should validate input at web page and lift reminder once a input is invalid. Also we have to consider correlation between inputs like start date and end date, so we used jquery to limit selection of date.

(2) About passing argument

In this project, we used different ways to passing argument. Argument
can be passed via render or redirect function. We can also use href attribute in html file to achieve redirecting with parameters. Another way is to store variables using session such that other function can use the stored variables in the same session.

(3) About procedure of design

After this project, we are more familiar with the procedure of building
a website and its application, especially we feel that we must be careful
when design a conceptual model for business and database because a good
design can really save time in building website page and alteration for logic mistakes. Moreover, we think there will always be problems we can't predict when designing database models in advance but encounter in practice.
