# JS2PDFInjector
Use this tool to Inject a JavaScript file into a PDF file.

To do this you will need an existing PDF file, and a ".js" file which contains the commands you would like to run when the document is opened.
I use this to create PDFs with some active code in there that I can email to customers, or download through their proxies, to check for the JavaScript being removed or blocked.
When you open the produced PDF with the JavaScript injected in Adobe you should see your code execute. Mileage varys in other viewers and for certain Edge in Windows 10 does not execute.

## JavaScript API available in PDF

The following URL contains the JavaScript API details:

http://www.adobe.com/content/dam/Adobe/en/devnet/acrobat/pdfs/js_api_reference.pdf

These are different from browser based JavaScript so reading that to achieve anything advanced is recommended.

## Example JavaScript

Who doesn't love a simple alert message? I know I do. The following code is all you will need for your hello world alert message:

```app.alert("Hello world!");```

A little different from just "alert". Your alert method now lives attached to the "app" object. Nothing too crazy.

## How to use

Follow the steps below to create your PDF:

* Execute the jar file. 
* If no command line arguments are provided "usage" instructions will be provided 
* Then it will open a GUI prompt that asks you to point it at a PDF file. 
* Select the PDF file you would like to inject in to.
* Another prompt will appear looking for the file containing your JavaScript.
* Select that file.
* This will create a new file in the same directory as the PDF with "js_injected_" prepended in the name.

To automate the process via the command line the following shows the new usage:

```java -jar JS2PDFInjector.jar <PDF FILE> <JS FILE>```

This does no checking that the files you provide exist so don't run with scissors.

If you need to inject one JS file into multiple PDF files then you can do so with a for loop. For bash that is shown below:

```
for pdf in /path/to/pdfs/*.pdf
do;
java -jar JS2PDFInjector.jar $pdf /path/to/javascript.js
done;
```

I have not spent time on crafting the interface. But this should work.

## Common Error ##

When running in headless mode if you see a FileNotFoundException like this:

```
[*] Original PDF: dummy.pdf
[*] JavaScript Payload: test.js
[*] Output File Path: null/js_injected_dummy.pdf
java.io.FileNotFoundException: null/js_injected_dummy.pdf (No such file or directory)
	at java.base/java.io.FileOutputStream.open0(Native Method)
	at java.base/java.io.FileOutputStream.open(FileOutputStream.java:299)
	at java.base/java.io.FileOutputStream.<init>(FileOutputStream.java:238)
	at java.base/java.io.FileOutputStream.<init>(FileOutputStream.java:188)
	at org.apache.pdfbox.pdmodel.PDDocument.save(PDDocument.java:1305)
	at com.cornerpirate.js2pdfinjector.JS2PDFInjector.main(JS2PDFInjector.java:107)
```

It means you used relative paths to the files. Please supply the absolute path as command line arguments. 
So instead of this command:

```
java -jar JS2PDFInjector.jar dummy.pdf test.js
```

You should use absolute paths like this:

```
java -jar JS2PDFInjector.jar /tmp/dummy.pdf /tmp/test.js
```

Full paths for the win.

## How to use JS2PDFInjector to Test your Email or Internet filtering

For more information please see the blog post by the author:

https://cornerpirate.com/2017/02/10/js2pdfinjector-javascript-for-your-pdf/

The blog post is no outdated and shows only the GUI usage path.

## Dislaimer

For research purposes only, do not use this on any target which you do not have permission to do so.
