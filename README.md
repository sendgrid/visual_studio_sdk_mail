# SendGrid SDK Extension #
This library allows you to quickly and easily send emails through SendGrid using Visual Studio SDK from your Windows 8 Applications in C# and Javascript and Windows Phone 8 Applications.
 
## License ##
Licensed under the MIT License.

## Downloading ##

Installing the SendGrid SDK Extension is as simple as executing SendGridSdkWindows8.vsix file for Windows 8 Projects or the SendGridSdkWindowsPhone.vsix for Windows Phone 8 Projects and including them as references in your projects.
The Windows Phone versions uses Json.Net library which is included in the VSIX installer.


If you're using git, you can just clone down the repo like this:

```
git clone git@github.com:sendgrid/visual_studio_sdk.git
```

__Note__: If you don't have git or would rather install by unpacking a Zip or Tarball, you can always grab the latest version of the package from [the downloads page](https://github.com/sendgrid/visual_studio_sdk/archive/master.zip). 


## SendGrid API ##
SendGridSDK provides SendGrid library in C# for sending emails from Windows 8 and Windows Phone 8 Apps.


## Mail Pre-Usage ##

Before we begin using the library, its important to understand a few things about the library architecture...

* The SendGrid object is the means of setting mail data. In general, data can be set in three ways for most elements:
  1. set - reset the data, and initialize it to the given element. This will destroy previous data
  2. set (List) - for array based elements, we provide a way of passing the entire array in at once. This will also destroy previous data.
  3. add - append data to the list of elements.

* Sending an email is as simple as :
  1. Creating a SendGrid object, and setting its data
  2. Sending the mail.

## Mail Usage ##

To begin using this library, you must first install the SDK


Then, initialize the SendGrid object with your SendGrid credentials

```JavaScript
var SendGrid = new SendGridSDK.Mail("<sendgrid_username>","<sendgrid_password>");
```

Add your message details

```JavaScript
SendGrid.setTo("foo@bar.com")
  .setFrom("me@bar.com")
  .setSubject("Subject goes here")
  .setText("Hello World!")
  .setHtml("<strong>Hello World!</strong>");
```

Send the email

 in Javascript:
```JavaScript
SendGrid.send().then(function (results) { console.log(results); });
```
 in C#:
```C#
String results = await SendGrid.send();
```
### Using Categories ###

You can mark messages with optional categories to give better visibility to email statistics (opens, clicks, etc.). You can add up to 10 categories per email message. You can read more about Categories here: http://docs.sendgrid.com/documentation/delivery-metrics/categories/

To add categories to your message, use the SendGrid.addCategory() method and pass a category as parameter or SendGrid.setCategories() and pass a list of category names. SendGrid will begin tracking statistics with these category names if the category name is new, or aggregate statistics for existing category names.

```JavaScript
SendGrid.addTo("foo@bar.com")
  ...
  .addCategory("Category 1")
  .addCategory("Category 2");
```

```JavaScript
SendGrid.addTo("foo@bar.com")
  ...
  .setCategories('["Category 1","Category 2","Category 3"]');
```

### Using Substitutions ###

SendGrid also allows you to send multi-recipient messages with unique information per recipient. This is commonly used for sending unique URLs or codes to a list of recipients in a single batch. You can read more about Substitutions here: http://docs.sendgrid.com/documentation/api/smtp-api/developers-guide/substitution-tags/

```JavaScript
SendGrid.addTo("john@somewhere.com")
  .addTo("harry@somewhere.com")
  .addTo("Bob@somewhere.com")
  ...
  .setHtml("Hey %name%, we've seen that you've been gone for a while")
  .addSubstitution("%name%", '["John", "Harry", "Bob"]');
```

### Using Sections ###

Used in conjunction with Substitutions, Sections can be used to further customize messages for the end users, and acts like a second tier of substitution data. You can use SendGrid.addSection() to add a single section, or SendGrid.setSections() method to add several sections. You can read more about using Sections here: http://docs.sendgrid.com/documentation/api/smtp-api/developers-guide/section-tags/

```JavaScript
SendGrid.addTo("john@somewhere.com")
  .addTo("harry@somewhere.com")
  .addTo("Bob@somewhere.com")
  ...
  .setHtml("Hey %name%, you work at %place%")
  .addSubstitution("%name%", '["John", "Harry", "Bob"]')
  .addSubstitution("%place%",'["%office%", "%office%", "%home%"]')
  .addSection("%office%", "an office")
  .addSection("%home%", "your house");
```

### Using Unique Arguments ###

Unique Arguments are used for tracking purposes on the message, and can be seen in the Email Activity screen on your account dashboard or through the Event API. Use the SendGrid.addUniqueArgument() method, which takes two parameters, a key and a value. To pass multiple keys/values, use SendGrid.setUniqueArguments() and pass a dictionary of key/value pairs. More information can be found here: http://docs.sendgrid.com/documentation/api/smtp-api/developers-guide/unique-arguments/

```JavaScript
SendGrid.addTo("foo@bar.com")
  ...
  .addUniqueArgument("Customer", "Someone")
  .addUniqueArgument("location", "Somewhere");
```

### Using Filter Settings ###

Filter Settings are used to enable and disable apps, and to pass parameters to those apps. You can read more here: http://docs.sendgrid.com/documentation/api/smtp-api/filter-settings/
Here's an example of passing content to the 'footer' app:

```JavaScript
SendGrid.addTo("foo@bar.com")
  ...
  .addFilterSetting("footer", "enable", "1")
  .addFilterSetting("footer", "text/plain", "Here is a plain text footer")
  .addFilterSetting("footer", "text/html", "<p style='color:red;'>Here is an HTML footer</p>");
```

### Using Bcc ###

Bcc is used to send a blind carbon copy to an address

```JavaScript
SendGrid.setBcc("foo@bar.com")
```

Note : addBcc() was removed because is currently not supported.
