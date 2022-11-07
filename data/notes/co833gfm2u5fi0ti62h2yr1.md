
# XML Transformations

Free tester tool: https://xslttest.appspot.com/

## Sorting XML Records with XSLT

https://dellboomi.force.com/community/s/article/Sorting-XML-Records-with-XSLT

Let's say that we have an XML document that looks like this:

```xml
<?xml version="1.0" encoding="utf-8"?>
<entityList>
    <createdDate>2020-03-02T05:02:03.000-08:00</createdDate>
    <entity internalId="376531302" lastModifiedDate="2020-03-03T05:51:35.000-08:00">
        <entityName>99001 ABC - Entity A</entityName>
    </entity>
    <entity internalId="37651304" lastModifiedDate="2020-03-02T04:37:24.000-08:00">
        <entityName>99006 ABC - Entity D</entityName>
    </entity>
    <entity internalId="376531302" lastModifiedDate="2020-03-02T04:19:16.000-08:00">
        <entityName>99005 ABC - Entity C</entityName>
    </entity>
    <entity internalId="376531302" lastModifiedDate="2020-03-20T06:44:22.000-08:00">
        <entityName>99002 ABC - Entity B</entityName>
    </entity>
</entityList>
```

This is the final processed form of our data, but the user we're sending this to has asked that we sort it by the "elementName" value.
We'll use these two templates in an XSLT Stylesheet for that.

```xml
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
<xsl:output method="xml" indent="yes" />

     <xsl:template match="entityList">
        <xsl:copy>
            <xsl:apply-templates select="@*"/>
            <xsl:apply-templates select="entity">
                <xsl:sort select="entityName" order="ascending"/>
            </xsl:apply-templates>
        </xsl:copy>
    </xsl:template>

    <xsl:template match="@* | node()">
        <xsl:copy>
            <xsl:apply-templates select="@* | node()"/>
        </xsl:copy>
    </xsl:template>

</xsl:stylesheet>
```