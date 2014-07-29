# Understanding the Miva Merchant Architecture / Ecosystem

It’s important that we understand how Store Morph Technology works. By knowing the theory and proper application of the templating system, it will be much easier to solve complex problems in the future.

Before you can learn how to customize a Miva Merchant store, it is essential that you learn the vocabulary and the basics of Miva Merchant so you have a solid foundation to build upon. This article contains a lot of concepts that are extremely important to understand and fully comprehend before diving head first into customizing a Miva Merchant store. The concepts and vocabulary described in this article will give you the background you need to understand how Miva Merchant templates are structured and the different elements they contain that give Miva Merchant it’s design flexibility.

## [Understanding Store Morph Technology](http://www.mivamerchant.com/videos/articles/dts-102-article-understanding-store-morph-technology)

Store Morph Technology (SMT), is the term used to describe the Merchant Merchant’s Page Templates and the components that make them up. SMT is made up of Pages, Items, Entities, and the Miva Merchant Template Lanuage. Miva Merchant Template Language which gives you the ability do conditional if-else statements and more complex programming constructs such as foreach loops and text string operators. You’ll learn more about the Templating Language in a future article.

### Pages

The core of customizing a Miva Merchant store starts with Pages. Pages are individual templates that define the layout of each webpage in a Miva Merchant Store. A Page is a mix of standard HTML, Items, Entities, and Miva Merchant conditional statements (foreach loops, if-else statements, etc.). You can also put things like CSS or JavaScript on a Miva Merchant Page. As of this writing, a default store has 53 page templates (PR8 Update 10) however new pages are constantly added as Miva adds new features. This includes everything from the SFNT page (Storefront) to the ORHL (Order History Lookup). It is very important to understand what each of the pages controls and when it is called within Miva Merchant.

See the [Page Reference Guide](http://www.mivamerchant.com/tutorials/articles/dts-102-article-page-reference-guide) to view the complete list of standard Miva Pages with a description of when each is used.

In Miva Merchant, the words Screen and Page are used interchangeably. If you are looking at a standard Miva Merchant URL you will always know what Page Template you are on by looking at the Screen=SFNT parameter in the URL. This tells you the Page Template that was rendered was the SFNT (Storefront) Page.
You also have the ability to create new Pages that contain any elements you want. This allows you to move all your content into Miva Merchant so that every page shares the same global elements. Miva Merchant not only acts as your shopping cart, but also as your Content Management System for your entire store. Common Pages to move into Miva Merchant are the about us page, contact us, policy pages, testimonials and FAQ’s.
Pseudo Pages
These are Pages in Miva Merchant that don’t have Page Templates associated with them in the Miva Merchant Admin. An example of this would be the OINF page. It is used to initiate the checkout process however it does not have its own page template. Instead, the OINF page has logic built in that will direct you to either the OCST Page (Customer Information Page) if you are logged in or the ORDL Page (Order Login Page) if you are not logged in. This functionality is all built into the Pseudo Page, OINF.

Luckily there are only 4 Pseudo Pages in Miva: OINF, ACNT, AFAE and OUSL. A detailed description of each can be found in the Page Reference Guide.
Items
Items can be one of the hardest concepts in Miva Merchant to grasp, however they are extremely important to how Miva Merchant Pages function.
An Item is a user interface component that provides specific functionality to a Page template.  All Pages have multiple items assigned to them. Some examples of commonly used items would be the nav bar, the category tree, and the head tag. Each of these are built in items that are assigned to almost all Pages of the store.

Items are the building blocks of Miva Merchant. Think of them as “includes” that can contain functions, HTML, and variables. For example, if you see the Global Header Item called on the page, all it is really doing is retrieving the content from that tab and printing it on the page.
There can be global items such as the nav bar, cat tree and head tag or there can be page specific items such as the Page header/footer. Once an item is assigned to a page you then call in the item through the item tag.
Item Tags can have 3 forms:
Self closing Item with no parameter.
<mvt:item name="html_profile" />
Self closing Item with parameter.
<mvt:item name="head" param="css_list" />
Item with opening and closing tags:
<mvt:item name="body">
</mvt:item>
When an item is assigned to a page, a lot of times a new tab will show up that will allow you to access the content of that item. If a tab has an asterisk next to it that means it is a global item and any changes you make the template will affect all Pages of the site that use that item.
Sometimes an item can be assigned to a page and no tab will show up. This does not mean that the item is not working correctly; it just means that the item does not have a tab or template you can access. Instead, the item may be assigned to give you access to specific variables on that page. An example of this would be the store item. When assigned to a page there is no new tab that shows up. Instead, it gives you access to global variables called Entities that allow us to print data like the Store Name.
Example:
<title>Welcome to &mvt:store:name;</title>
There is an Items tab for each page in Miva Merchant which will show you exactly which items are assigned to that page, and also which ones aren’t assigned.
Items Assigned to a Page
Items Assigned to a Page

One pitfall that new developers run into frequently is forgetting to assign non-default items to pages when they want to access them. After spending a while debugging, they will find out that all they had to do was assign them item!
The real power of Items comes from the ability for 3rd party module developers to create new items that extend Miva Merchant’s core functionality.  An item is just a reference to a Miva Merchant Component Module (See Miva’s API for more information on this). A Component Module is a compiled Miva Script file that follows Miva Merchant’s API. This allows developers to leverage the power of the Miva Script programming language to create almost any functionality imaginable all within a very flexible and highly secure framework.
Item Diagram
Illustration of Item Diagram.
Entities
Entities are variables that can be used throughout a Miva Merchant Page template to display a value on the screen.  Depending on which items you have assigned to a page, you have access to different entities that you can use to write a value to the screen.
Entity Examples
All entities start with the ampersand ( & ) and end with a semicolon ( ; )
&mvt:store:name;  - Prints the store name
&mvte:category:name; - Prints the current category name
&mvt:product:formatted_price; - Prints the formatted price of the product ($10.99)
The first part, &mvt, tells us this is a Miva Merchant page template entity. The middle part, like store or category tells us what part of the store interface is being referenced.(Which item is being referenced).
The last part, like name, or formatted_price, gives the specific piece of information to use from the item being referenced.

An entity can contain different types of data, from integers to text strings to image urls.
When outputting an Entity to the screen you have different encoding methods to choose from.
How do I know which one I should use?
&mvt – Prints the value directly to the screen with no encoding.
&mvte - Variables that begin with &mvte are “entity encoded.” All characters are encoded so they are not interpreted by the browser. This is used for all form input values and anywhere user input is written back to the page. Entity encoding variables prevents against cross side scripting and other harmful attacks.
&mvta - Variables that begin with &mvta are “attribute encoded.” This means that any characters they contain will be converted to the correct format for use in a link. This is used for all links and will convert spaces and other characters to link friendly characters.
