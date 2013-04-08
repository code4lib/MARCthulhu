= Provenance =

This file contains two records from widely distributed Library of Congress 
bibliographic metadata, including via [archive.org] [1].

The source files are v36.i01.records.xml and v36.i40.records.xml.

= Pathology =

The records include some properly represented Arabic script, but the real problem is that they are invalid MARCXML, 
putting the numeric character entity "&#27;" (apostrophe)  as an 880 subfield code.  Valid values (a-z, 0-9) are defined 
by [LoC] [2].

The records are particularly problematic because they cause certain XML parsers to crash, notably including marc4j.
Thus they are a good test of your system.

= Example =

A marc4j crash on one of the files yields:

    [Fatal Error] :422256:28: Character reference "&#
    Caught: org.marc4j.MarcException: Unable to parse input
    org.marc4j.MarcException: Unable to parse input
            at org.marc4j.MarcXmlParser.parse(MarcXmlParser.java:95)
            at org.marc4j.MarcXmlParser.parse(MarcXmlParser.java:64)
            at org.marc4j.MarcXmlParserThread.run(MarcXmlParserThread.java:115)
    Caused by: org.xml.sax.SAXParseException; lineNumber: 422256; columnNumber: 28; Character reference "&#
            at com.sun.org.apache.xerces.internal.parsers.AbstractSAXParser.parse(AbstractSAXParser.java:1236)
            at com.sun.org.apache.xerces.internal.jaxp.SAXParserImpl$JAXPSAXParser.parse(SAXParserImpl.java:568)
            at org.marc4j.MarcXmlParser.parse(MarcXmlParser.java:93)

Here are the files:lines:content in question, as output by grep -n:

    v36.i01.records.xml:422256:      <subfield code="&#27;">(3maBeirut :</subfield>
    v36.i40.records.xml:671210:      <subfield code="&#27;">(3naشايسته‌فر، مهناز.</subfield>

The former line is semantically incorrect, since "maBeirut" is not Arabic script (as designated by the '(3' prefix).


= Links =
    [1]: http://archive.org/details/marc_loc_updates ""
    [2]: http://www.loc.gov/marc/bibliographic/bd880.html  "LOC Bibliographic Definition of 800 field"
