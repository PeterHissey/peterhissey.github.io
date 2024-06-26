<h2 id="h_01HF42RKB6E2YR0RQ4RYZHV71V">
  <strong><span class="wysiwyg-color-blue120">Introduction</span></strong>
</h2>
<p>
  This document describes the Media Operations Platform REST API, which is the
  interface by which a third-party component can make REST calls over HTTP to the
  Platform application server endpoint and receive responses from it. It does not
  cover any of the following topics:
</p>
<ul>
  <li>
    Web Hooks – please refer to the following article called
    <a href="/hc/en-us/articles/20338332222107" target="_blank" rel="noopener noreferrer">Webhooks user guide 10.4.&nbsp;</a>
  </li>
  <li>The Platform SOAP API</li>
  <li>Platform Service Order Adapters</li>
  <li>
    Other outbound interfaces, such as alternate authentication systems.<br>
    Although most of this document is relevant to the v1 API, it also includes
    features that only exist in the v2 API and are primarily intended for users
    of the v2 API.
  </li>
</ul>
<h2 id="h_01HKT2AK41QD6G450RPS4XSMM6">
  <span class="wysiwyg-color-orange">Summary of what's new in v10.6</span>
</h2>
<ul>
  <li>
    Swagger documentation now includes the
    <a href="/hc/en-us/articles/21789230412571#h_01HF36JJ5WR17BWBNHKZZCCQR1">PATCH method using a POST payload</a>.
  </li>
  <li>
    Swagger includes example payloads for sub-tables (previously not populated).
  </li>
  <li>
    Swagger now includes
    <a href="/hc/en-us/articles/21789230412571#h_01HF40NRXV8NZWCRJ5PV9TMSZ1">PATCH method for list documents.</a>
  </li>
  <li>
    Other Swagger improvements and correct missing parameter fields.
  </li>
  <li>
    Additional header
    <a href="/hc/en-us/articles/21789230412571#h_01HKSPJS614F0QQG81TQQDCZ24">parameters for optimization and filtering</a>
    <ul>
      <li>
        <a href="/hc/en-us/articles/21789230412571#h_01HKSQ234JHJYHVREZWD7C867W">Ignore alternate keys</a>
      </li>
      <li>
        <a href="/hc/en-us/articles/21789230412571#h_01HKSQ339VZTSRBSD25DCPTR8A">Omit null values</a>
      </li>
      <li>
        <a href="/hc/en-us/articles/21789230412571#h_01HF434Z8JY77TBZKP9TZXEN8A">Filter results for maintenance documents</a>
      </li>
    </ul>
  </li>
</ul>
<h2 id="h_01HF30DJ7M98Z65ZT21DNGVQZH">
  <strong><span class="wysiwyg-color-blue120">Definitions</span></strong>
</h2>
<table style="border-collapse: collapse; width: 100%; height: 802px;" border="1">
  <tbody>
    <tr style="height: 22px;">
      <td class="wysiwyg-text-align-center" style="width: 15.7143%; height: 22px;">
        <span class="wysiwyg-color-black40"><strong>Platform</strong></span>
      </td>
      <td class="wysiwyg-text-align-center" style="width: 84.2857%; height: 22px;">
        <span class="wysiwyg-color-black40"><strong>Media Operations Platform application&nbsp;</strong></span>
      </td>
    </tr>
    <tr style="height: 67px;">
      <td class="wysiwyg-text-align-center" style="width: 15.7143%; height: 67px;">API</td>
      <td style="width: 84.2857%; height: 67px;">
        Application Program Interface is a set of functions or procedures
        offered by a host system that allows one or more applications, services,
        or systems to send information to, request information from, or make
        changes to the host system.
      </td>
    </tr>
    <tr style="height: 89px;">
      <td class="wysiwyg-text-align-center" style="width: 15.7143%; height: 89px;">REST</td>
      <td style="width: 84.2857%; height: 89px;">
        Representational State Transfer is an architectural style of principles
        that define a set of constraints for how two systems can communicate
        over a network such as the World Wide Web. The REST architecture
        is described in
        <a href="/hc/en-us/articles/20339398168219#h_01HF387Q6QAWSQ2EZMVZPAYQ1J">REST Basics</a>
      </td>
    </tr>
    <tr style="height: 67px;">
      <td class="wysiwyg-text-align-center" style="width: 15.7143%; height: 67px;">Verb</td>
      <td style="width: 84.2857%; height: 67px;">
        In REST, this is the programmatic method used to initiate an API
        command, usually GET, PUT, POST, or DELETE. See also
        <a href="/hc/en-us/articles/20339398168219#h_01HF42TTJMW7WJRZ5CQ9GZMFSB">HTTP Methods</a>
        for more information.
      </td>
    </tr>
    <tr style="height: 67px;">
      <td class="wysiwyg-text-align-center" style="width: 15.7143%; height: 67px;">
        <br>
        Swagger
      </td>
      <td style="width: 84.2857%; height: 67px;">
        A third-party tool that generates API documentation automatically
        based on the Open API specification. It presents a browsable, usable
        Web site as a URL within the API Web service. See also Open API.
      </td>
    </tr>
    <tr style="height: 134px;">
      <td class="wysiwyg-text-align-center" style="width: 15.7143%; height: 134px;">cURL</td>
      <td style="width: 84.2857%; height: 134px;">
        cURL is a mechanism for transferring data using networking protocols.
        It is provided in two ways:<br>
        • As a command line tool that allows a user to submit requests directly
        and see the results, usually for testing purposes.<br>
        • As a library with integrations to several common programming languages,
        including C++, C#, PHP, and others.
      </td>
    </tr>
    <tr style="height: 156px;">
      <td class="wysiwyg-text-align-center" style="width: 15.7143%; height: 156px;">Model</td>
      <td style="width: 84.2857%; height: 156px;">
        A collection of properties, usually defined as name/value pairs or
        collections of name/value pairs, that make up a document or a portion
        of a document. An API may accept one or more models as input, to
        create a new record or modify an existing record using all the specified
        values. An API may also provide one or more models as output, either
        as a list of identical models (e.g., retrieving multiple records
        as part of a list document) or as a collection of related models
        (e.g., retrieving all models related to a particular Job document).
      </td>
    </tr>
    <tr style="height: 89px;">
      <td class="wysiwyg-text-align-center" style="width: 15.7143%; height: 89px;">Postman</td>
      <td style="width: 84.2857%; height: 89px;">
        <p>
          A third-party platform for building and testing APIs; the Postman
          client application is available on several operating systems
          and provides a set of tools to test API calls without having
          to write code and helps debug API calls and response payloads.
          <a href="https://www.postman.com/">https://www.postman.com/</a>
        </p>
      </td>
    </tr>
    <tr style="height: 111px;">
      <td class="wysiwyg-text-align-center" style="width: 15.7143%; height: 111px;">
        OAS,<br>
        OpenAPI
      </td>
      <td style="width: 84.2857%; height: 111px;">
        Formerly known as Swagger Specification, the OpenAPI Specification
        (OAS) defines a standard interface to RESTful APIs. This language-agnostic
        interface can then be used by code generation, documentation generation,
        and testing tools to access and understand the underlying API.&nbsp;<br>
        Refer to
        <a href="https://swagger.io/specification/" target="_blank" rel="noopener noreferrer">https://swagger.io/specification/</a>
        for more information.
      </td>
    </tr>
  </tbody>
</table>
<h2 id="h_01HF37ATBNZDTSG1JDW62N8MEP">
  <strong><span class="wysiwyg-color-blue120">API Basics</span></strong>
</h2>
<p>
  This section describes the fundamental ideas behind APIs, the REST protocol,
  and the portions of the Platform system involved.
</p>
<p>
  The purpose of an application program interface (API) is to allow an external
  computer or piece of software (here called an endpoint) to interact with the
  system that exposes the interface (here called the host), to allow the endpoint
  to do one or more of the following:
</p>
<ul>
  <li>Create new records in the host system</li>
  <li>Retrieve information from the host system</li>
  <li>Update existing records in the host system</li>
  <li>Delete existing records in the host system</li>
  <li>Trigger operations or business logic in the host system</li>
</ul>
<p>
  Unlike a user interface, an API is a mechanism for one piece of software to talk
  to another and usually does not require the action of a person. In some cases,
  however, an API is used to allow a third-party user interface to interact with
  the host system.
</p>
<h4 id="h_01HF387Q6QAWSQ2EZMVZPAYQ1J">
  <strong><span class="wysiwyg-color-blue120">REST Basics</span></strong>
</h4>
<p>
  Representational State Transfer (REST) is an architectural style of principles
  that define a set of constraints for how two systems can communicate over a network
  such as the World Wide Web. These principles are used to create reliable Web
  APIs where no state information needs to be retained by the host system offering
  the API.
</p>
<h4 id="h_01HF3885WHG4DD15DXJ7TCXDR9">
  <strong><span class="wysiwyg-color-blue120">Features and Benefits</span></strong>
</h4>
<p id="01HFASN0YVKEK0TS4J8525SYDA">The REST architecture provides the following benefits:</p>
<ul>
  <li style="list-style-type: none;">
    <ul>
      <li>
        Simple – In general, REST calls follow a set of common patterns that
        can be learned and followed without significant amounts of special
        syntax.
      </li>
      <li>
        Stateless - Being “stateless” allows the API provider to handle high
        volumes of requests with high performance.
        <ul>
          <li>
            The server is not required to keep state information in memory.
          </li>
          <li>
            The client is not required to connect to a specific server
            to utilize stored state information, so requests can be distributed
            among any number of servers.
          </li>
        </ul>
      </li>
    </ul>
  </li>
</ul>
<h4 id="h_01HF42TTJMW7WJRZ5CQ9GZMFSB">
  <strong><span class="wysiwyg-color-blue120">HTTP Methods</span></strong>
</h4>
<p id="01HFASN0YVAE80C6XQVDBRTMDM">
  RESTful services utilize HTTP methods to differentiate between different types
  of API calls.
</p>
<p>&nbsp;</p>
<table style="border-collapse: collapse; width: 100%; height: 154px;" border="1">
  <tbody>
    <tr style="height: 22px;">
      <td class="wysiwyg-text-align-center" style="width: 11.8571%; height: 22px;">
        <span class="wysiwyg-color-black wysiwyg-color-black40"><strong>Verb</strong></span>
      </td>
      <td class="wysiwyg-text-align-center" style="width: 88.1429%; height: 22px;">
        <span class="wysiwyg-color-black40"><strong>Communi</strong><strong>ty Used For</strong></span>
      </td>
    </tr>
    <tr style="height: 22px;">
      <td class="wysiwyg-text-align-center" style="width: 11.8571%; height: 22px;">GET&nbsp;</td>
      <td style="width: 88.1429%; height: 22px;">
        Usually retrieves a representation of the resource’s current state,
        such as performing a simple query that gathers and returns information
        about one or more resources (e.g., database records) from the host
        system.<br>
        In the Platform API, GET commands are used to retrieve setup documents,
        maintenance documents, or list documents.<br>
        Examples include:<br>
        Retrieving a list of all records of a certain document type, such
        as all Bids, all Jobs, or all Contacts in the system,
        <ul>
          <li>
            Retrieving a list of all records of a certain document type
            that match specified criteria, such as all Bids that start
            in the current year, all Jobs associated with a specific
            Client, or all Contacts whose names begin with the letter
            A, or
          </li>
          <li>
            Retrieving a specific record of a certain document type,
            such as all the properties of a Job with a specific Job ID.
          </li>
        </ul>
        Note: A GET command is entirely contained within the URI sent to
        the endpoint. As a result, there is a limit on the amount of text
        that can be sent to the API, so GET commands usually do not support
        a significant amount of search or filter criteria.
      </td>
    </tr>
    <tr style="height: 22px;">
      <td class="wysiwyg-text-align-center" style="width: 11.8571%; height: 22px;">PATCH</td>
      <td style="width: 88.1429%; height: 22px;">
        Patch usually updates an existing state with a specified one using
        partial information. See also PUT to completely replace an existing
        state, and POST for updating an existing state in some cases. In
        the Platform API, usually updates an existing record, such as<br>
        changing the mailing address of a Contact or changing the status
        of a Job.
      </td>
    </tr>
    <tr style="height: 22px;">
      <td class="wysiwyg-text-align-center" style="width: 11.8571%; height: 22px;">POST</td>
      <td style="width: 88.1429%; height: 22px;">
        Post usually processes the representation provided with the request
        to create a new representation or update an existing representation.&nbsp;<br>
        In the Platform API, POST commands are usually used to create new
        records, such as adding a new resource or creating a new Job.
      </td>
    </tr>
    <tr style="height: 22px;">
      <td class="wysiwyg-text-align-center" style="width: 11.8571%; height: 22px;">PUT</td>
      <td style="width: 88.1429%; height: 22px;">
        Put usually updates an existing record by completely replacing an
        existing state with a specified one or creating a new state where
        the URI is already known. See also PATCH to replace only a portion
        of an existing state. In the Platform API, PUT commands will be used
        to perform upsert calls from v11; refer to POST commands for creating
        new items or PATCH commands for updating existing items.
      </td>
    </tr>
    <tr style="height: 22px;">
      <td class="wysiwyg-text-align-center" style="width: 11.8571%; height: 22px;">DELETE</td>
      <td style="width: 88.1429%; height: 22px;">
        Delete commands are usually used to permanently remove existing records
        from the host system.&nbsp;<br>
        Note: DELETE commands should be used sparingly; to both preserve
        data integrity and provide historical information, it is usually
        recommended to change the Status of a record instead of deleting
        the record completely.
      </td>
    </tr>
  </tbody>
</table>
<p>&nbsp;</p>
<p id="h_01HF388DTR1XD2GA70P2DNNNJN">
  <strong><span class="wysiwyg-color-black">Response Codes</span></strong>
</p>
<p>
  REST services are built on top of the HTTP protocol, so calls to the REST service
  generate HTTP response codes. In many cases, the HTTP response code will be accompanied
  by additional information in the header or body of the message. In some cases,
  the HTTP response code may be the only response.
</p>
<p>&nbsp;</p>
<table style="border-collapse: collapse; width: 100%; height: 286px;" border="1">
  <tbody>
    <tr style="height: 22px;">
      <td class="wysiwyg-text-align-center" style="width: 28.1429%; height: 22px;">
        <span class="wysiwyg-color-black40"><strong>Response Code&nbsp;</strong></span>
      </td>
      <td class="wysiwyg-text-align-center" style="width: 71.8571%; height: 22px;">
        <span class="wysiwyg-color-black40"><strong>Description</strong></span>
      </td>
    </tr>
    <tr style="height: 22px;">
      <td style="width: 28.1429%; height: 22px;">
        <strong>2xx range&nbsp;</strong>
      </td>
      <td style="width: 71.8571%; height: 22px;">
        <strong>Success Codes</strong>
      </td>
    </tr>
    <tr style="height: 22px;">
      <td style="width: 28.1429%; height: 22px;">200 Ok</td>
      <td style="width: 71.8571%; height: 22px;">
        The call was successful. In most cases, there will be additional
        information returned by the API in the body of the message, such
        as the matching record(s) for a query, or the ID of a record created,
        modified, or deleted by a corresponding API call.
      </td>
    </tr>
    <tr style="height: 22px;">
      <td style="width: 28.1429%; height: 22px;">204 No Content</td>
      <td style="width: 71.8571%; height: 22px;">
        The server has fulfilled the request but does not need to return
        a response body. The server may return the updated meta information.
      </td>
    </tr>
    <tr style="height: 22px;">
      <td style="width: 28.1429%; height: 22px;">
        <strong>4xx range&nbsp;</strong>
      </td>
      <td style="width: 71.8571%; height: 22px;">
        <strong>Client Error</strong>
      </td>
    </tr>
    <tr style="height: 22px;">
      <td style="width: 28.1429%; height: 22px;">400 Bad Request</td>
      <td style="width: 71.8571%; height: 22px;">
        The call was not successful due to an error in the URL or the syntax
        of the API call. Check the syntax of the API call to make sure there
        are no invalid characters.
      </td>
    </tr>
    <tr style="height: 22px;">
      <td style="width: 28.1429%; height: 22px;">401 Unauthorized</td>
      <td style="width: 71.8571%; height: 22px;">
        The call was not successful because the API requires a valid<br>
        credentials, and they were not provided as part of the call.<br>
        Response Code Description Verify that the call is providing login
        information in an acceptable manner.
      </td>
    </tr>
    <tr style="height: 22px;">
      <td style="width: 28.1429%; height: 22px;">
        404 Not Found<br>
        <br>
      </td>
      <td style="width: 71.8571%; height: 22px;">
        The call was not successful because the service could not find the
        requested API. Check the URL for any mismatches between the API call
        being sent and the documented API signature.
      </td>
    </tr>
    <tr style="height: 22px;">
      <td style="width: 28.1429%; height: 22px;">
        <strong>5xx range&nbsp;</strong>
      </td>
      <td style="width: 71.8571%; height: 22px;">
        <strong>Server Error</strong>
      </td>
    </tr>
    <tr style="height: 22px;">
      <td style="width: 28.1429%; height: 22px;">500 Internal Server Error</td>
      <td style="width: 71.8571%; height: 22px;">
        The call was not successful because it caused an error in the service
        during processing, such as providing an incorrect data type for a
        given property/field. Verify that all parameters are correct, and
        values are valid.
      </td>
    </tr>
    <tr style="height: 22px;">
      <td style="width: 28.1429%; height: 22px;">502 Bad Gateway</td>
      <td style="width: 71.8571%; height: 22px;">
        The call was not successful because the service got an invalid response
        from the API. Verify that all parameters are correct, and values
        are valid.
      </td>
    </tr>
  </tbody>
</table>
<p>
  <br>
  For more information refer to the
  <a href="https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html" target="_blank" rel="noopener noreferrer">appropriate section of the HTTP protoco</a>l
  or a developer resource for HTTP status codes, such as the
  <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Status" target="_blank" rel="noopener noreferrer">MDN Web Docs.</a>
</p>
<h2 id="h_01HF32REA2J4DJPMWZXC2KYZ3S">
  <strong><span class="wysiwyg-color-blue120">Xytech Platform REST API</span></strong>
</h2>
<p>
  The Platform is, at a fundamental level, a system that deals with creating, updating,
  and utilizing a particular type of data object referred to as a document.
</p>
<ul>
  <li>
    Each Platform document is a representation of a database table or collection
    of database tables.
    <ul>
      <li>
        Each document has a primary table and may have one or more sub-tables.
      </li>
      <li>
        All sub-tables are children of the primary table, and a sub-table
        can have one or more child sub-tables.
      </li>
    </ul>
  </li>
  <li>
    Each document is one of the following types of documents:
    <ul>
      <li>
        <strong>Setup -</strong> generally describes a single item, and usually
        only contains a primary table. Setup documents are often used to
        manage simple items used to generate lists of options in other documents,
        such as status labels or predefined sets of codes.
      </li>
      <li>
        <strong>Maintenance</strong> – generally describes either master
        data (which are used in transactional data) or transactional data.
        Maintenance documents often contain one or more sub-tables.
      </li>
      <li>
        <strong>List</strong> documents provide access to sets of other records,
        such as Setup and Maintenance documents.
      </li>
    </ul>
  </li>
</ul>
<p>
  The Platform REST API is JSON-based and has Open API v3.0 API specifications
  for each API call available in YAML. These specifications can be retrieved as
  a plain YAML file and are also readily available to be viewed in a browser through
  the Swagger UI. See the Error! Reference source not found. below for more information.
</p>
<h2 id="h_01HKT98RYPP3DHJQ2DKFE36W49">
  <strong><span class="wysiwyg-color-blue120">High level data model diagram</span></strong>
</h2>
<p>
  This diagram provides you with a high-level understanding of the primary Xytech
  Platform data objects and how they relate.
</p>
<p id="h_01HF32S3WPY6XMNRBKXBN6KM9N">
  Below the name of each data object is the REST API endpoint used for accessing
  the data object. Where ~/ is prefixed before the endpoint, that indicates it
  is a sub-table endpoint of the primary endpoint. See the Swagger documentation
  for full details.
</p>
<h2 id="h_01HKT98RYPAXQ05FW167ZTW2FC">
  <strong><span class="wysiwyg-color-blue120"><img src="/guide-media/01HHH9X552WEPZK1WH3EED2T25" alt="MediaPulse primary data objects-Summary.drawio.png"></span></strong>
</h2>
<h2 id="h_01HKT98RYPVQ8BVQREJHY949MX">
  <strong><span class="wysiwyg-color-blue120">REST API v1 and v2</span></strong>
</h2>
<h4 id="h_01HF41NX3EMVWH3XRG1VE0P6D7">
  <strong><span class="wysiwyg-color-blue120">Deprecation of v1 API</span></strong>
</h4>
<ul>
  <li>
    REST API v1 was the first Xytech REST API product. It had limitations that
    required breaking changes to improve so v2 was made available from Xytech
    v9.4 release with improved functionality.
  </li>
  <li>
    REST API v1 will no longer be supported in version 11.0 and beyond.
  </li>
  <li>
    All new integrations should use v2 API and existing integrations should port
    to v2
  </li>
  <li>
    To use v2 API, change the base URL version number from ../v1/.. to ../v2/..
  </li>
</ul>
<h4 id="h_01HF37D08VJRETQVS03SKK2Q24">
  <strong><span class="wysiwyg-color-blue120">Additional Features in v2 API (as of Platform v10.2 release)</span></strong>
</h4>
<ul>
  <li>
    <strong>Payload structure is enhanced and updated to be more robust and scalable.</strong><br>
    The document name is now always added as the root element of the payload
    with an array containing the details.&nbsp;
  </li>
</ul>
<p>
  <br>
  <span class="wysiwyg-color-black40"><em>Example showing the additional root element jm_job:<br></em></span>
</p>
<table style="border-collapse: collapse; width: 100.303%;" border="1">
  <tbody>
    <tr>
      <td class="wysiwyg-text-align-center" style="width: 47.0215%;">
        <strong>API V1 JSON payload for GET JmJob</strong>
      </td>
      <td style="width: 59.8284%;">
        <p class="wysiwyg-text-align-center">
          <strong>API V2 JSON payload for GET JmJob</strong>
        </p>
      </td>
    </tr>
  </tbody>
</table>
<table style="border-collapse: collapse; width: 100%;" border="1">
  <tbody>
    <tr>
      <td style="width: 26.3503%; vertical-align: top;">
        <p>/JmJob/job_no=11261</p>
        <div>
          <div>
            <img src="/guide-media/01HKQB8KXQK48725XBS3X4QYAA" alt="Picture4.png">
          </div>
        </div>
      </td>
      <td style="width: 29.1926%; vertical-align: top;">
        <p>/JmJob/job_no=11261</p>
        <div>
          <div>
            &nbsp;
            <img src="/guide-media/01HKQB9GXQF9PWQ6EAXQ4D85NW" alt="Picture5.png" width="305" height="310">
          </div>
        </div>
      </td>
    </tr>
  </tbody>
</table>
<ul>
  <li style="list-style-type: none;">
    <ul>
      <li>
        See the Swagger documentation for details of the JSON structure for
        each endpoint.
      </li>
    </ul>
  </li>
  <li>
    <strong>Additional PATCH capabilities<br></strong>In addition to the existing
    v1 PATCH method where each changed field must be defined using the operation/path/value
    payload structure, you can now use the same JSON payload you would use for
    a POST call to update a record. This needs to use the header 'Content-Type:
    <span class="wysiwyg-color-orange110">application/json-patch+json'</span>.
    <a href="/hc/en-us/articles/21789230412571#h_01HF36JJ5WR17BWBNHKZZCCQR1">See the section below for more details.</a>
  </li>
</ul>
<ul>
  <li>
    <strong>Pagination &amp; sorting<br></strong>API Pagination has been added
    to the REST API ‘Get’ Query for Lists.<br>
    <span class="wysiwyg-color-black40"><strong>Parameters for:</strong></span>
    <table style="border-collapse: collapse; width: 100%;" border="1">
      <tbody>
        <tr>
          <td style="width: 5.19412%;">
            <p>
              <strong>pageSize&nbsp;</strong>
            </p>
          </td>
          <td style="width: 61.4725%;">are the number of records returned per page.&nbsp;</td>
        </tr>
        <tr>
          <td style="width: 5.19412%;">
            <strong>page</strong>
          </td>
          <td style="width: 61.4725%;">are the page number to return.&nbsp;</td>
        </tr>
        <tr>
          <td style="width: 5.19412%;">
            <p>
              <strong>sort</strong>&nbsp;
            </p>
          </td>
          <td style="width: 61.4725%;">
            is the field to sort by followed by ascending or descending
            order.&nbsp;
          </td>
        </tr>
      </tbody>
    </table>
    Example:<br>
    To return the first 10 records on page 1 sorted by product_no:<br>
    GET<br>
    {server}}/JmOrgProductList?query="active":"Y"}&amp;resultColumns="L":"product_no","product_desc"]}&amp;sort="product_no_desc"]&amp;pageSize=10&amp;page=1<br>
    <br>
    This will allow sites to call for data in manageable payloads without exceeding
    memory limitations.
  </li>
</ul>
<h2 id="h_01HF33ASVQ3PKX7Y1NW9N9PK3V">
  <strong><span class="wysiwyg-color-blue120">Connecting to the REST API</span></strong>
</h2>
<h4 id="h_01HF41M0E3PDN5T1YJM5EWRWZM">
  <strong><span class="wysiwyg-color-blue120">Licensing</span></strong>
</h4>
<p>
  While there is a specific license point for the REST API, by default all Platform
  installations are given access to the API.
</p>
<p>
  The REST API is a component of the Platform’s Application Server. On hosted systems,
  it is available by default to all customers.
</p>
<h4 id="h_01HF41MJD3GWA4EQTWWMSM2TG3">
  <strong><span class="wysiwyg-color-blue120">Instance Information You Will Need</span></strong>
</h4>
<p>
  To be able to connect with the REST API, you will need to know the instance:
</p>
<ul>
  <li>
    Base URL e.g. ‘https://example.com’ (The same base url you use to access
    your system)
  </li>
  <li>Database name e.g. ‘DEMO1’</li>
</ul>
<p>
  For systems installed prior to mid 2023, you may also require the port number.
  All later systems are now setup with a proxy to avoid the need to know the port
  number.
</p>
<p>
  If you do not have this information, please contact Xytech Technical Support.
</p>
<h2 id="h_01HF41MS17YF27BJVTKYA39KWA">
  <strong><span class="wysiwyg-color-blue120">Swagger API Documentation</span></strong>
</h2>
<p>
  From Platform version v10.2, a Swagger index page has been introduced to assist
  in the navigation and creation of Swagger YAML documentation. The Swagger index
  page can be found by entering the base URL you use for accessing the Platform
  and adding “/ApiDocs” to the end (e.g., www.xytechexample.com/XYT_TEST/ApiDocs).
  The Xytech API Index page is displayed.&nbsp;
</p>
<p>&nbsp;</p>
<p>
  <img src="/guide-media/01HF3Z41GTRGCYZNP50W6JZ89V">
</p>
<p>
  <strong>Using the Index</strong><br>
  The Index is comprised of the following areas:
</p>
<ul>
  <li>
    Version: Displayed in the upper left corner of the screen. This shows the
    API version, currently either v1 or v2.
  </li>
  <li>
    Selected Database: The name of the database currently selected for this host.<br>
    Only modules for that database are shown.
  </li>
  <li>
    Available Databases: (Where multiple databases exist on the same host) Click
    a database to select it and view its associated modules.
  </li>
  <li>
    Filter Documents: Enter text into this field and press [Enter] to only display
    matching document descriptions. To clear the filter, delete the text and
    press [Enter].
  </li>
  <li>
    Module and Document List: Only modules in the selected database are displayed.
    Click any Module to expand and show the documents it contains. Each document
    has a label that describes its type (Document [maintenance], List, or Setup).
  </li>
</ul>
<h4 id="h_01HKQC6C7JAT5FFXYR0HEDHPRV">
  <strong>For versions prior to v10.2</strong>
</h4>
<p id="01HKQC717B5ZDV7CGWW0AD794R">
  For versions prior to v10.2 you will need to generate the Swagger documentation
  manually for each document endpoint.
</p>
<p>Use a browser to generate the Swagger document:</p>
<p class="wysiwyg-indent1">
  <span class="wysiwyg-color-blue">http://{host}:{port}/API/v2/database/{db_name}/spec/{docName}</span>
</p>
<p>For example:</p>
<p class="wysiwyg-indent1">
  <span class="wysiwyg-color-blue">http://myhost:8088/API/v2/database/mp10/spec/JmJob</span>
</p>
<p>
  The above URL will generate the Swagger document and then display the Swagger
  document.
</p>
<p>To display generated Swagger documents, browse to:</p>
<p class="wysiwyg-indent1">
  <span class="wysiwyg-color-blue">http://{host}:{port}/REST/SwaggerUI/dist/index.html?document={docName}_v2</span>
</p>
<p>For example:</p>
<p class="wysiwyg-indent1">
  <span class="wysiwyg-color-blue">http://myhost:8088/REST/SwaggerUI/dist/index.html?document=JmDivision_v2</span>
</p>
<h2 id="h_01HF432A2QDTNAGSRW2W8MPY6Y">
  <strong><span class="wysiwyg-color-blue120">Authentication</span></strong>
</h2>
<p>
  The REST API currently uses Basic Authentication and will require a database
  login account.<br>
  Always use HTTPS encrypted protocol when communicating with the REST API to ensure
  credentials are not passed in clear text.
</p>
<h2 id="h_01HF432H4G52KGXS00QMNA032D">
  <strong><span class="wysiwyg-color-blue120">Using Postman as a test client</span></strong>
</h2>
<p>
  This section describes using the third-party Postman application as an API client
  for testing and troubleshooting purposes. The example uses the JmJob (Job) endpoint.
</p>
<p>
  <br>
  <strong>To execute a GET call to retrieve an existing record:</strong>
</p>
<ol>
  <li>Open the Postman application</li>
  <li>From File click New and click HTTP Request</li>
  <li>Leave the method as GET</li>
  <li>
    Enter the URL:<br>
    <ul>
      <li>
        <span class="wysiwyg-color-blue">http://{base_url}/API/v2/database/{dbname}/jmJob/job_no=100&nbsp;</span>
        <ul>
          <li>
            Replace {base_url} &amp; {dbname} with your base URL &amp;
            database name and provide a valid Job Number that exists
            in your instance.
          </li>
        </ul>
      </li>
    </ul>
  </li>
  <li>
    On the Authorization tab, set Type to Basic Auth and enter a valid username
    and password, e.g. xytech/xytechpw
  </li>
  <li>Click Send and observe the response.</li>
</ol>
<p>
  <strong>To execute a POST call to create a new record:</strong>&nbsp;
</p>
<ol>
  <li>Open the Postman application.</li>
  <li>
    From <strong>File</strong> click <strong>New</strong> and click
    <strong>HTTP Request</strong>.
  </li>
  <li>
    Change the method to <strong>POST</strong>.
  </li>
  <li>
    Add a Header:
    <ul>
      <li>
        Key: <strong>Content-Type</strong>
      </li>
      <li>
        Value: <strong>application/json</strong>
      </li>
    </ul>
  </li>
  <li>
    Enter the URL: http://{base_url}/API/v2/database/{dbname}/jmJob
  </li>
  <li>
    On the Authorization tab, set <strong>Type</strong> to Basic Auth and enter
    a valid username and password, e.g. xytech/xytechpw
  </li>
  <li>
    On the Body tab, set the type to<strong> json,</strong> and provide a valid
    JSON object with the minimum fields to create a job. For example, to create
    a simple Job:
  </li>
</ol>
<div>
  <div>
    <table style="border-collapse: collapse; width: 100%;" border="1">
      <tbody>
        <tr>
          <td style="width: 100%;">
            <div>
              <div>
                <span style="color: #000000;">{</span>
              </div>
              <div>
                <span style="color: #000000;">&nbsp; &nbsp; </span><span style="color: #0451a5;">"jm_job"</span><span style="color: #000000;">: [</span>
              </div>
              <div>
                <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; {</span>
              </div>
              <div>
                <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"cust_id"</span><span style="color: #000000;">: {</span>
              </div>
              <div>
                <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"list_id"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"409"</span>
              </div>
              <div>
                <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; },</span>
              </div>
              <div>
                <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"job_desc"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"Passing Fancy"</span><span style="color: #000000;">,</span>
              </div>
              <div>
                <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"job_no"</span><span style="color: #000000;">: {</span>
              </div>
              <div>
                <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"job_no"</span><span style="color: #000000;">: </span><span style="color: #098658;">-1</span>
              </div>
              <div>
                <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; },</span>
              </div>
              <div>
                <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"job_type_no"</span><span style="color: #000000;">: {</span>
              </div>
              <div>
                <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"job_type_no"</span><span style="color: #000000;">: </span><span style="color: #098658;">11</span>
              </div>
              <div>
                <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; }</span>
              </div>
              <div>
                <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; }</span>
              </div>
              <div>
                <span style="color: #000000;">&nbsp; &nbsp; ]</span>
              </div>
              <div>
                <span style="color: #000000;">}</span>
              </div>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
  <div>&nbsp;</div>
</div>
<div>
  <strong>NOTES</strong>:
</div>
<ul>
  <li style="list-style-type: none;">
    <ul>
      <li>
        To generate the primary key job_no, set the value to -1.<br>
        When creating multiple sub-records in a single call, subsequent records
        must advance the primary key ID by -1. E.g -1, -2, -3 etc…
      </li>
      <li>
        Be aware some fields are mandatory such as Job Type (job_type_no)
        and Customer ID (list_id).
      </li>
      <li>Numeric values will be accepted with or without quotes.</li>
    </ul>
  </li>
</ul>
<p>
  &nbsp; &nbsp; &nbsp; 8. Click <strong>Send</strong> and observe the response
  with status code 200 (success with response body).
</p>
<div>
  The id of the created record is returned in a header parameter called 'Location'.
</div>
<div>
  <em>In v10.6 SP1 a response payload will contain the ID(s) of newly created records.</em>
</div>
<div>&nbsp;</div>
<p>
  <strong>To execute a DELETE call to remove an existing record:</strong>
</p>
<ol>
  <li>Open the Postman application.</li>
  <li>
    From <strong>File</strong> click <strong>New</strong> and click
    <strong>HTTP Request</strong>.
  </li>
  <li>
    Change the method to <strong>DELETE</strong>.
  </li>
  <li>
    Enter the URL:<br>
    <span class="wysiwyg-color-blue">http://{base_url}/api/v2/database/{dbname}/JmJob/job_no=67982</span>
  </li>
  <li>
    On the Authorization tab, set <strong>Type</strong> to Basic Auth and enter
    a valid username and password, e.g. xytech/xytechpw
  </li>
  <li>
    Click <strong>Send</strong> and observe the response with status code 204
    (success with no response body).
  </li>
</ol>
<h4 id="h_01HF4339DX1SYCDK60DYCK93ZA">
  <strong><span class="wysiwyg-color-blue120">Using cURL as a test client</span></strong>
</h4>
<p>
  This section describes using the third-party cURL command line utility as an
  API client for testing and troubleshooting purposes.&nbsp;
</p>
<p>
  Many REST clients (either via an online tool or desktop application) use a lightweight
  program called curl to interact over many different types of network protocols
  (including HTTP, HTTPS, FTP, IMAP, etc.) Windows users may use the curl command
  from the Command Prompt to call teh REST API and output dumps of useful data.
</p>
<p>Curl is included with Windows v10 1803 and later.</p>
<p>
  <em>Example curl on Windows to fetch job details and download the response to a file (replace credentails, base url and database name with your values</em>:
</p>
<pre id="h_01HKQMD40FVN94KJ6AYKSJ7G7Q">curl -u xytech:password <a href="https://{base">https://{base_</a>url}/api/v2/database/{database name}/JmJob/job_no=342 -H "Accept: application/json" &gt;c:\temp\ApiResponse.txt</pre>
<h2 id="h_01HKQAS67BD284R245T31D2GKV">
  <span class="wysiwyg-color-blue120"><strong>Key fields in the payload structure</strong></span>
</h2>
<ul>
  <li>
    The nested sub-group identified by the primary key field name, provides all
    the key fields of a document. As a minimum, this sub-group always contains
    the primary key field and an 'external key' field.
  </li>
</ul>
<p>
  <em>Example shows a section of the Work Order's (jm_work_order) key fields in the sub-group wo_no_seq.</em>
</p>
<table style="border-collapse: collapse; width: 100%;" border="1">
  <tbody>
    <tr>
      <td style="width: 100%;">
        <div>
          <div>
            <span style="color: #000000;">{</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; </span><span style="color: #0451a5;">"jm_work_order"</span><span style="color: #000000;">: [</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; {</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"wo_no_seq"</span><span style="color: #000000;">: {</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"wo_no_seq"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"2627-1"</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"external_key"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; },</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"custom_field_data_no"</span><span style="color: #000000;">: </span><span style="color: #098658;">63888</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"wo_desc"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"Dscription2"</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"wo_desc_2"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">...</span>
          </div>
        </div>
      </td>
    </tr>
  </tbody>
</table>
<ul>
  <li style="list-style-type: none;">
    <ul>
      <li>
        &nbsp;In the above example
        <span class="wysiwyg-color-red120">wo_no_seq </span>and
        <span class="wysiwyg-color-red120">external_key </span>are the two
        key fields that can be used in lookup requests.
      </li>
      <li>
        Some documents may have additional key fields such as Media Assets
        (known by the document name lib_master) See example below.
      </li>
    </ul>
  </li>
</ul>
<p>
  <em><span class="wysiwyg-color-black40">Example Media Asset document where there are additional key fields for 'barcode' &amp; 'umid' in the 'master_no' group.</span></em>
</p>
<table style="border-collapse: collapse; width: 100%;" border="1">
  <tbody>
    <tr>
      <td style="width: 100%;">
        <div>
          <div>
            <span style="color: #000000;">{</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; </span><span style="color: #0451a5;">"lib_master"</span><span style="color: #000000;">: [</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; {</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"master_no"</span><span style="color: #000000;">: {</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"master_no"</span><span style="color: #000000;">: </span><span style="color: #098658;">53</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"barcode"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"1838"</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"external_key"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"umid"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; },</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"cust_id"</span><span style="color: #000000;">: {</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"list_id"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"23"</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"other_cust_id"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"cust_reference"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"external_key"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"cust_id"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"23"</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; },</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"customer_name"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"Gotta Dance Productions"</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"master_desc"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"A Winter's Tale"</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"barcode"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"1838"</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"container_master_no"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">...</span>
          </div>
        </div>
      </td>
    </tr>
  </tbody>
</table>
<ul>
  <li>
    External key fields exist on all documents and are designed to be used by
    external systems to store the external system's unique identifier for the
    record. The external key field can then be used in future look-ups as opposed
    to the external system having to know the Xytech key field value..
  </li>
</ul>
<h2 id="h_01HF433TKM2GMHVRE5CPR8X3X6">
  <strong><span class="wysiwyg-color-blue120">Example Postman Collection calls</span></strong>
</h2>
<p>
  An extensive set of example API calls is available via the
  <a href="https://www.postman.com/xytech-product-team/workspace/xytech-platform-public/collection/25646735-19fb63a8-470d-460d-9eea-d23248239302?action=share&amp;creator=25646735" target="_blank" rel="noopener noreferrer">Platform’s REST API Postman Collection</a>.
  This collection contains example calls for three types of documents (maintenance,
  list, and setup).&nbsp;
</p>
<p>
  We suggest forking the collection to your own workspace which enables you to
  'pull' for updates as teh collection grows.
</p>
<h2 id="h_01HKSM9RNVXQ9ESQEWDTMJMT2S">
  <span class="wysiwyg-color-blue120"><strong>Additional details (supplemental the Swagger documentation)</strong></span>
</h2>
<h2 id="h_01HWNFSGCW6RMVN3VDNWR22PS9">
  <strong><span class="wysiwyg-font-size-large wysiwyg-color-blue120">PATCH - update datetime fields to 'null'</span></strong>
</h2>
<p id="h_01HWNFPJW3Y1GXMF40SYKDT46W">
  It may seem like an obvious approach to use the 'update' operation with a value
  of 'null' to clear a datetime field, but datetime fields will not get set to
  null in that way.
</p>
<p>
  The only way to 'null' a datetime field is by using the 'remove' operation.
</p>
<p>
  Example PATCH payload that set's a datetime field to 'null':
</p>
<div style="color: #000000; background-color: #ffffff; font-family: Consolas, 'Courier New', monospace; font-weight: normal; font-size: 14px; line-height: 19px; white-space: pre;">
  <div>
    <span style="color: #000000;">[</span>
  </div>
  <div>
    <span style="color: #000000;">&nbsp; &nbsp; {</span>
  </div>
  <div>
    <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"op"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"remove"</span><span style="color: #000000;">,</span>
  </div>
  <div>
    <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"path"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"goodnight_date"</span><span style="color: #000000;">,</span>
  </div>
  <div>
    <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"value"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span>
  </div>
  <div>
    <span style="color: #000000;">&nbsp; &nbsp; }</span>
  </div>
  <div>
    <span style="color: #000000;">]</span>
  </div>
</div>
<p>&nbsp;</p>
<h2 id="h_01HF36KBMD33F7GGS9YV7NZD27">
  <strong><span class="wysiwyg-color-blue120">Query parameters</span></strong>
</h2>
<p>
  <strong>GET</strong> and <strong>PATCH</strong> List endpoints support query
  parameters.
</p>
<p>
  This section describes the syntax and options used for the query parameter. This
  is a mandatory parameter for List documents.
</p>
<p>
  The general format for a “query” parameter is to add the parameter as
  <em>query={}</em> to the end of a GET request for a List document after the parameter
  delimiter (“?”), where the value of query= is a JSON object:
</p>
<p class="wysiwyg-indent1">
  <span class="wysiwyg-color-blue">http://{base_url}/documentList?query={key: value}</span>
</p>
<p>Such as:</p>
<p class="wysiwyg-indent1">
  <span class="wysiwyg-color-blue">http://{base_url}/jmJobList?query={"job_no": "12345"<a href="http://base_url/jmJobList?query={&quot;job_no&quot;:&quot;12345&quot;}">}</a></span>
</p>
<p>
  <strong>Note:</strong> The query parameter is supported only for GET and PATCH
  requests for List type documents and is <strong>not</strong> supported by GET
  requests for Setup or Maintenance documents.
</p>
<p class="MsoNormal">
  <span lang="EN-US">In the simplest form, the value is a single piece of information, such as a string or integer. In more complex forms, the value is a JSON object containing specific formats as described below.</span>
</p>
<p>
  In these examples below, the <em>baseurl</em> is assumed to include the document
  endpoint<br>
  e.g ‘<span class="wysiwyg-color-blue"><a href="https://example.com/api/v2/database/example/JmJobList’">https://example.com/api/v2/database/example/JmJobList’</a></span>
</p>
<p>&nbsp;</p>
<h3 id="h_01HY164T3J13GEVCTEYR228P1G" class="wysiwyg-indent2">TIP: URL encoding of special characters e.g. % and +</h3>
<p class="wysiwyg-indent2">
  When using HTML special characters as part of the query value, they must be URL
  encoded.
</p>
<div class="wysiwyg-indent2" style="box-sizing: border-box; color: #000000; font-family: ' Segoe UI VSS (Regular)' , ' Segoe UI' , -apple-system, BlinkMacSystemFont, Roboto, ' Helvetica Neue' , Helvetica, Ubuntu, Arial, sans-serif, ' Apple Color Emoji' , ' Segoe UI Emoji' , ' Segoe UI Symbol' font-size:14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">
  Example:&nbsp;<span style="box-sizing: border-box; color: rgba(0, 0, 0, 0.9); font-family: ' Segoe UI VSS (Regular)' , ' Segoe UI' , -apple-system, BlinkMacSystemFont, Roboto, ' Helvetica Neue' , Helvetica, Ubuntu, Arial, sans-serif, ' Apple Color Emoji' , ' Segoe UI Emoji' , ' Segoe UI Symbol' font-size:15px; display: inline !important;"><span style="box-sizing: border-box; color: rgba(0, 0, 0, 0.9); font-family: ' Segoe UI VSS (Regular)' , ' Segoe UI' , -apple-system, BlinkMacSystemFont, Roboto, ' Helvetica Neue' , Helvetica, Ubuntu, Arial, sans-serif, ' Apple Color Emoji' , ' Segoe UI Emoji' , ' Segoe UI Symbol' font-size:15px; display: inline !important;"><span style="box-sizing: border-box;">&nbsp;to use a wildcard query such as</span> "<strong style="box-sizing: border-box;">%dave%"</strong>, the % needs substituting with </span></span><strong style="box-sizing: border-box;">%25</strong>.
  <span style="box-sizing: border-box; color: rgba(0, 0, 0, 0.9); font-family: ' Segoe UI VSS (Regular)' , ' Segoe UI' , -apple-system, BlinkMacSystemFont, Roboto, ' Helvetica Neue' , Helvetica, Ubuntu, Arial, sans-serif, ' Apple Color Emoji' , ' Segoe UI Emoji' , ' Segoe UI Symbol' font-size:15px; display: inline !important;">&nbsp;Once URL encoded will look like this <strong style="box-sizing: border-box;">%25dave%25</strong></span>
</div>
<p class="wysiwyg-indent2" style="box-sizing: border-box; color: #000000; font-family: ' Segoe UI VSS (Regular)' , ' Segoe UI' , -apple-system, BlinkMacSystemFont, Roboto, ' Helvetica Neue' , Helvetica, Ubuntu, Arial, sans-serif, ' Apple Color Emoji' , ' Segoe UI Emoji' , ' Segoe UI Symbol' font-size:14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">Example GET query with URL encoded wildcard :</p>
<p class="wysiwyg-indent2" style="box-sizing: border-box; color: #000000; font-family: ' Segoe UI VSS (Regular)' , ' Segoe UI' , -apple-system, BlinkMacSystemFont, Roboto, ' Helvetica Neue' , Helvetica, Ubuntu, Arial, sans-serif, ' Apple Color Emoji' , ' Segoe UI Emoji' , ' Segoe UI Symbol' font-size:14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">
  {{server}}/MoMediaOrderList?query={"wo_desc":"<span style="box-sizing: border-box; color: rgba(0, 0, 0, 0.9); font-family: ' Segoe UI VSS (Regular)' , ' Segoe UI' , -apple-system, BlinkMacSystemFont, Roboto, ' Helvetica Neue' , Helvetica, Ubuntu, Arial, sans-serif, ' Apple Color Emoji' , ' Segoe UI Emoji' , ' Segoe UI Symbol' font-size:15px; display: inline !important;"><strong style="box-sizing: border-box;">%25dave%25</strong></span>"}&amp;resultcolumns={"L":
  ["wo_no", "wo_desc"]}
</p>
<div class="wysiwyg-indent2" style="box-sizing: border-box; color: #000000; font-family: ' Segoe UI VSS (Regular)' , ' Segoe UI' , -apple-system, BlinkMacSystemFont, Roboto, ' Helvetica Neue' , Helvetica, Ubuntu, Arial, sans-serif, ' Apple Color Emoji' , ' Segoe UI Emoji' , ' Segoe UI Symbol' font-size:14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">
  <em>(Specifically the reason why&nbsp;<strong style="box-sizing: border-box;">%dave%</strong> fails to return valid results it that <strong style="box-sizing: border-box;">%da</strong>&nbsp;is the encoding for the<strong style="box-sizing: border-box;">&nbsp;Ú</strong> character)</em>
</div>
<div class="wysiwyg-indent2" style="box-sizing: border-box; color: #000000; font-family: ' Segoe UI VSS (Regular)' , ' Segoe UI' , -apple-system, BlinkMacSystemFont, Roboto, ' Helvetica Neue' , Helvetica, Ubuntu, Arial, sans-serif, ' Apple Color Emoji' , ' Segoe UI Emoji' , ' Segoe UI Symbol' font-size:14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">&nbsp;</div>
<div class="wysiwyg-indent2" style="box-sizing: border-box; color: #000000; font-family: ' Segoe UI VSS (Regular)' , ' Segoe UI' , -apple-system, BlinkMacSystemFont, Roboto, ' Helvetica Neue' , Helvetica, Ubuntu, Arial, sans-serif, ' Apple Color Emoji' , ' Segoe UI Emoji' , ' Segoe UI Symbol' font-size:14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">
  This also applies to datetime values that use the offset attribute with the +
  sign.
</div>
<div class="wysiwyg-indent2" style="box-sizing: border-box; color: #000000; font-family: ' Segoe UI VSS (Regular)' , ' Segoe UI' , -apple-system, BlinkMacSystemFont, Roboto, ' Helvetica Neue' , Helvetica, Ubuntu, Arial, sans-serif, ' Apple Color Emoji' , ' Segoe UI Emoji' , ' Segoe UI Symbol' font-size:14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">
  To include a value of "<strong>2023-06-01T09:00+5:00</strong>" in a URL query
  parameter, substitute + with <strong>%2b</strong>
</div>
<div class="wysiwyg-indent2" style="box-sizing: border-box; color: #000000; font-family: ' Segoe UI VSS (Regular)' , ' Segoe UI' , -apple-system, BlinkMacSystemFont, Roboto, ' Helvetica Neue' , Helvetica, Ubuntu, Arial, sans-serif, ' Apple Color Emoji' , ' Segoe UI Emoji' , ' Segoe UI Symbol' font-size:14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">Example:</div>
<div class="wysiwyg-indent2" style="box-sizing: border-box; color: #000000; font-family: ' Segoe UI VSS (Regular)' , ' Segoe UI' , -apple-system, BlinkMacSystemFont, Roboto, ' Helvetica Neue' , Helvetica, Ubuntu, Arial, sans-serif, ' Apple Color Emoji' , ' Segoe UI Emoji' , ' Segoe UI Symbol' font-size:14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">
  ?<span style="color: #000000; font-family: ' Segoe UI VSS (Regular)' , ' Segoe UI' , -apple-system, BlinkMacSystemFont, Roboto, ' Helvetica Neue' , Helvetica, Ubuntu, Arial, sans-serif, ' Apple Color Emoji' , ' Segoe UI Emoji' , ' Segoe UI Symbol' font-size:14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; background-color: #ffffff; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial; display: inline !important; float: none;">query={"wo_begin_dt":{"$range":["2023-06-01T09:00</span><strong><span style="box-sizing: border-box; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; letter-spacing: normal; orphans: 2; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; background-color: #ffffff; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial; color: #212121; font-family: Inter, system-ui, -apple-system, BlinkMacSystemFont, ' Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, ' Fira Sans', ' Droid Sans', Helvetica, Arial, sans-serif; font-size: 12px; text-align: left; display: inline !important;"><span style="box-sizing: border-box; font-size: 14px; text-align: start; display: inline !important;">%2b</span></span></strong><span style="color: #000000; font-family: ' Segoe UI VSS (Regular)' , ' Segoe UI' , -apple-system, BlinkMacSystemFont, Roboto, ' Helvetica Neue' , Helvetica, Ubuntu, Arial, sans-serif, ' Apple Color Emoji' , ' Segoe UI Emoji' , ' Segoe UI Symbol' font-size:14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; background-color: #ffffff; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial; display: inline !important; float: none;">5:00","2023-06-01T17:00</span><strong><span style="box-sizing: border-box; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; letter-spacing: normal; orphans: 2; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; background-color: #ffffff; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial; color: #212121; font-family: Inter, system-ui, -apple-system, BlinkMacSystemFont, ' Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, ' Fira Sans', ' Droid Sans', Helvetica, Arial, sans-serif; font-size: 12px; text-align: left; display: inline !important;"><span style="box-sizing: border-box; font-size: 14px; text-align: start; display: inline !important;">%2b</span></span></strong><span style="color: #000000; font-family: ' Segoe UI VSS (Regular)' , ' Segoe UI' , -apple-system, BlinkMacSystemFont, Roboto, ' Helvetica Neue' , Helvetica, Ubuntu, Arial, sans-serif, ' Apple Color Emoji' , ' Segoe UI Emoji' , ' Segoe UI Symbol' font-size:14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; background-color: #ffffff; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial; display: inline !important; float: none;">5:00"]}}</span>
</div>
<div style="box-sizing: border-box; color: #000000; font-family: ' Segoe UI VSS (Regular)' , ' Segoe UI' , -apple-system, BlinkMacSystemFont, Roboto, ' Helvetica Neue' , Helvetica, Ubuntu, Arial, sans-serif, ' Apple Color Emoji' , ' Segoe UI Emoji' , ' Segoe UI Symbol' font-size:14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">&nbsp;</div>
<div class="wysiwyg-text-align-left wysiwyg-indent2" style="box-sizing: border-box; color: #000000; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">
  <a href="https://www.w3schools.com/tags/ref_urlencode.ASP" target="_blank" rel="noopener noreferrer">HTML URL Encoding Reference</a>
</div>
<ul>
  <li>
    <span class="wysiwyg-font-size-large"><strong>String or Number</strong></span>
  </li>
</ul>
<table style="border-collapse: collapse; width: 100%;" border="1">
  <tbody>
    <tr>
      <td style="width: 17.4286%;">Description&nbsp;</td>
      <td style="width: 82.5714%;">
        The value in returned items must match a specified string. The string<br>
        can either be letters or numbers. Wildcard ‘%’ can be used.&nbsp;
      </td>
    </tr>
    <tr>
      <td style="width: 17.4286%;">Syntax&nbsp;</td>
      <td style="width: 82.5714%;">“field”:” value”</td>
    </tr>
    <tr>
      <td style="width: 17.4286%;">
        <p>Examples:</p>
        <p>
          <em>like</em>:<br>
          <br>
        </p>
      </td>
      <td style="width: 82.5714%;">
        <p>
          <em>baseurl</em>?Query={“job_desc”:” Big Apple Live”}
        </p>
        <p>
          <em>baseurl</em>?Query={“job_desc”:”%Big%”}
        </p>
        <p>
          <em>baseurl</em>?Query={“job_no”:101101}
        </p>
      </td>
    </tr>
  </tbody>
</table>
<p>
  To specify multiple key/value pairs, separate each key/value pair with a comma:<br>
  <em>baseurl</em>?query={"cust_id":"123","job_type_no":"4"}<br>
  <em>baseurl</em>?query={"cust_id":"123","job_type_no":"4","active":"Y"}<br>
  Note: When specifying multiple key/value pairs, the API will return only items
  that match ALL specified criteria.<strong><br></strong>
</p>
<ul>
  <li>
    <span class="wysiwyg-font-size-large"><strong>Range</strong></span>
  </li>
</ul>
<table style="border-collapse: collapse; width: 100%;" border="1">
  <tbody>
    <tr>
      <td style="width: 16.4285%;">Description</td>
      <td style="width: 83.5715%;">
        &nbsp;The value in returned items must fall between a specified minimum<br>
        and maximum numeric value.
      </td>
    </tr>
    <tr>
      <td style="width: 16.4285%;">Syntax&nbsp;</td>
      <td style="width: 83.5715%;">“field”:{"$range":[lower_limit, upper_limit]}</td>
    </tr>
    <tr>
      <td style="width: 16.4285%;">Examples</td>
      <td style="width: 83.5715%;">
        baseurl?Query={“job_no”:{"$range":[100, 199]}}<br>
        baseurl?query={"wo_begin_dt":{"$range":["2023-12-01","2023-12-31"]}}<br>
        baseurl?query={"wo_begin_dt":{"$range":["2023-06-01T09:00","2023-06-<br>
        01T17:00"]}}
      </td>
    </tr>
  </tbody>
</table>
<ul>
  <li>
    <span class="wysiwyg-font-size-large"><strong>In (Set)</strong></span>
  </li>
</ul>
<table style="border-collapse: collapse; width: 100%;" border="1">
  <tbody>
    <tr>
      <td style="width: 16%;">Description&nbsp;</td>
      <td style="width: 84%;">
        The value in returned items must match one of the values provided
        in a given set of values.
      </td>
    </tr>
    <tr>
      <td style="width: 16%;">Syntax&nbsp;</td>
      <td style="width: 84%;">“field”:{"$in":[“value_1”,”value_2”, … ”value_n”]}</td>
    </tr>
    <tr>
      <td style="width: 16%;">Examples&nbsp;</td>
      <td style="width: 84%;">baseurl?Query={“job_no”:{"$in":[100, 105, 110, 119]}}</td>
    </tr>
  </tbody>
</table>
<p>&nbsp;</p>
<p>
  <strong>TIP: Searching for multiple wildcard values</strong>
</p>
<p>
  If you wanted to search for multiple wildcard values contained within a single
  field that all need to exist, then you can include a list of wildcard values.
</p>
<p>
  For example, if you wanted to search for media assets that have "Tale" AND "Dark"
  in their "master_desc" field, you can use the following query parameter:
</p>
<p>
  <span class="wysiwyg-color-red130">{"master_desc":{$in:["%Tale%Dark%","%Tale%Dark%"]}}</span>
</p>
<p>
  Or with encoding of the % symbol (if using Postman) you need:
</p>
<p>
  <span class="wysiwyg-color-red130">{"master_desc":{$in:["%25Tale%25Dark%25","%25Tale%25Dark%25"]}}</span>
</p>
<p>&nbsp;</p>
<ul>
  <li>
    <span class="wysiwyg-font-size-large"><strong>Null / Empty</strong></span>
  </li>
</ul>
<table style="border-collapse: collapse; width: 100%;" border="1">
  <tbody>
    <tr>
      <td style="width: 16.8571%;">Description&nbsp;</td>
      <td style="width: 83.1429%;">
        The value in returned items must be NULL. Note: Put pipe characters<br>
        around NULL to differentiate it from the literal string “NULL”.
      </td>
    </tr>
    <tr>
      <td style="width: 16.8571%;">Syntax&nbsp;</td>
      <td style="width: 83.1429%;">"field”: “|NULL|”&nbsp;</td>
    </tr>
    <tr>
      <td style="width: 16.8571%;">Examples&nbsp;</td>
      <td style="width: 83.1429%;">baseurl?Query={“phone_number”:{"|NULL|"}}</td>
    </tr>
  </tbody>
</table>
<ul>
  <li>
    <span class="wysiwyg-font-size-large"><strong>NE (Not Equal)</strong></span>
  </li>
</ul>
<table style="border-collapse: collapse; width: 100%;" border="1">
  <tbody>
    <tr>
      <td style="width: 17%;">Description&nbsp;</td>
      <td style="width: 83%;">
        The value in returned items must not match the specified number,
        string, or NULL.
      </td>
    </tr>
    <tr>
      <td style="width: 17%;">Syntax&nbsp;</td>
      <td style="width: 83%;">“field”:{"$ne":”value”}</td>
    </tr>
    <tr>
      <td style="width: 17%;">
        <p>
          Examples:<br>
          &nbsp;Not equal<br>
          &nbsp;Not null&nbsp;<br>
          &nbsp;Not like
        </p>
      </td>
      <td style="width: 83%;">
        <p>
          baseurl?Query={"job_desc ":{"$ne":"Big Apple Live"}}<br>
          baseurl?Query={"cust_id":{"$ne":1001}}<br>
          baseurl?Query={"jm_phase_external_key":{"$ne":"|NULL|"}}<br>
          baseurl?Query={"wo_desc":{"$ne":"Test%"},"wo_type_no": 83}&nbsp;
        </p>
      </td>
    </tr>
  </tbody>
</table>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p id="h_01HF407Z477AWRA2EWFDH7XDF0">
  <strong><span class="wysiwyg-color-black">NULL Values</span></strong>
</p>
<p>
  The Null parameter returns any record that has a null value for the specified
  key, which indicates that no value has ever been set. This differentiates it
  from an empty string for text-based or date-based properties, a 0 value for numbers,
  and true or false values for Boolean properties.
</p>
<p>
  <strong>Note:</strong> Not all properties support null values. If possible, check
  whether the database column in the appropriate column allows nulls.
</p>
<p>
  <strong>Keys</strong> are strings and generally match the column name from a
  given database table. It is<br>
  recommended to match the case of the key name as shown in the Swagger API documentation
  if possible.
</p>
<p>
  <strong>Values</strong>
</p>
<p>- String values are not case-sensitive.</p>
<p>
  - DateTime values should be provided in a valid ISO date format.
</p>
<h2 id="h_01HF434Z8JY77TBZKP9TZXEN8A">
  <strong><span class="wysiwyg-color-blue120">resultColumns parameter</span></strong>
</h2>
<p>
  <span class="wysiwyg-color-black">Used by the <strong>GET</strong> method on List and (v10.6) Maintenance endpoints.</span>
</p>
<p>
  ResultColumns parameter is used to define the fields you wish to have returned
  in the response.<br>
  Without this parameter, the response will contain all document fields.
</p>
<p>
  <em>For example:</em><br>
  <span class="wysiwyg-color-blue">{baseurl}/JmJobList?Query={"job_no":2}&amp;resultColumns={"L":["job_no","job_desc"]}</span>
</p>
<p>
  Job No. and Job Description fields will be included in the response.<br>
  Important to include the “L” top-level element.
</p>
<p>
  <strong>Sub-Table columns</strong>
</p>
<p>
  Many endpoints include related sub-tables in their responses. Example syntax
  to include specific sub-table columns.
</p>
<p>
  Below example fetches a tranmission order description and all it's service row
  numbers:
</p>
<p>
  <span class="wysiwyg-color-blue90">{base_url}/XmTransmissionOrder/wo_no_seq=7655-1?resultColumns={"jm_work_order":["wo_desc"],"mo_service_row":["service_row_no"]}</span>
</p>
<p>
  <em>Response:</em>
</p>
<table style="border-collapse: collapse; width: 100%;" border="1">
  <tbody>
    <tr>
      <td style="width: 100%;">
        <div>
          <div>
            <span style="color: #000000;">{</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; </span><span style="color: #0451a5;">"jm_work_order"</span><span style="color: #000000;">: [</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; {</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"wo_desc"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"WS Transmission Test"</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"mo_service_row"</span><span style="color: #000000;">: [</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; {</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"service_row_no"</span><span style="color: #000000;">: {</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"service_row_no"</span><span style="color: #000000;">: </span><span style="color: #098658;">9933</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"external_key"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; }</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; },</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; {</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"service_row_no"</span><span style="color: #000000;">: {</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"service_row_no"</span><span style="color: #000000;">: </span><span style="color: #098658;">9934</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"external_key"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; }</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; }</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ]</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; }</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; ]</span>
          </div>
          <div>
            <span style="color: #000000;">}</span>
          </div>
        </div>
      </td>
    </tr>
  </tbody>
</table>
<p>
  <strong>Performance recomendation.</strong>
  <span class="wysiwyg-underline">Always</span> use the ResultColumns parameter
  otherwise responses will return large numbers of fields most of which will not
  be required and only adds to the system perfromance overhead. In future API versions,
  this will become a mandatory parameter.
</p>
<h2 id="h_01HKSPJS614F0QQG81TQQDCZ24">
  <strong><span class="wysiwyg-color-blue120">Additional parameters</span></strong>
</h2>
<h4 id="h_01HF36DRPX0VSVY5PSTZY3C9MY">
  <strong><span class="wysiwyg-color-blue120">saveArgument</span></strong>
</h4>
<p>
  In some cases, you must also add a header with the SaveArgument parameter to
  trigger the app server to perform a function as part of an API call. For example,
  when creating a Work Order, to load a Work Order Template you must provide the
  wo_template_no value in the payload as well as set the SaveArguement header:
</p>
<p>
  LoadTemplate:<br>
  <strong>SaveArgument : {"LoadTemplate":"2"}</strong>
</p>
<p>&nbsp;</p>
<p>
  LoadServiceTemplate, loads a service template to a Media Order or a Transmission
  Order:
</p>
<p>
  <strong>SaveArgument : {"LoadServiceTemplate":"10"}</strong>
</p>
<p>&nbsp;</p>
<p>
  BidApproval, changes the bid approval state of a Bid using an number that maps
  to the approval
</p>
<h4 id="h_01HTG0TJYBG38SHP0XZRTPJN5G">
  <strong>SaveArgument : {"BidApproval":"0"}</strong>
</h4>
<p>Approval number mapping:</p>
<table style="border-collapse: collapse; width: 44.1429%;" border="1">
  <tbody>
    <tr>
      <td style="width: 5.71429%;">0</td>
      <td style="width: 38.4286%;">
        <span class="ui-provider a b c d e f g h i j k l m n o p q r s t u v w x y z ab ac ae af ag ah ai aj ak" dir="ltr">Approval</span>
      </td>
    </tr>
    <tr>
      <td style="width: 5.71429%;">1</td>
      <td style="width: 38.4286%;">
        <span class="ui-provider a b c d e f g h i j k l m n o p q r s t u v w x y z ab ac ae af ag ah ai aj ak" dir="ltr">Unapproval</span>
      </td>
    </tr>
    <tr>
      <td style="width: 5.71429%;">2</td>
      <td style="width: 38.4286%;">
        <span class="ui-provider a b c d e f g h i j k l m n o p q r s t u v w x y z ab ac ae af ag ah ai aj ak" dir="ltr">ApproveAsChangeMemo</span>
      </td>
    </tr>
    <tr>
      <td style="width: 5.71429%;">3</td>
      <td style="width: 38.4286%;">
        <span class="ui-provider a b c d e f g h i j k l m n o p q r s t u v w x y z ab ac ae af ag ah ai aj ak" dir="ltr">ApproveAndUnApproveOriginal</span>
      </td>
    </tr>
    <tr>
      <td style="width: 5.71429%;">4</td>
      <td style="width: 38.4286%;">
        <span class="ui-provider a b c d e f g h i j k l m n o p q r s t u v w x y z ab ac ae af ag ah ai aj ak" dir="ltr">Abort</span>
      </td>
    </tr>
  </tbody>
</table>
<p>&nbsp;</p>
<h4 id="h_01HKSQ234JHJYHVREZWD7C867W">
  <strong><span class="wysiwyg-color-blue120">alternatekeyhandling</span></strong>
</h4>
<p class="wysiwyg-text-align-justify">
  Suppresses additional key fields from the responses. If you do not need to work
  with key fields other than the primary key, use this parameter to keep the API
  call performant and reduce the processing overhead when not working with alternate
  key fields (v10.6).
</p>
<p>
  Applicable to all GET calls for List, Maintenance &amp; Report endpoints.
</p>
<p>
  Values are ‘<strong>ignore</strong>’ or ‘<strong>include</strong>’.
</p>
<p>
  Default value = ‘include’ (includes alternate keys in the response)
</p>
<p>Example:</p>
<p>
  <span class="wysiwyg-color-black60"><em>URL Parameter:- alternatekeyhandling:<strong>include</strong> (default)&nbsp;</em></span>
</p>
<table style="border-collapse: collapse; width: 100%;" border="1">
  <tbody>
    <tr>
      <td style="width: 100%;">
        <div>
          <div>
            <span style="color: #000000;">{</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; </span><span style="color: #0451a5;">"L"</span><span style="color: #000000;">: [</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; {</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"master_no"</span><span style="color: #000000;">: {</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"master_no"</span><span style="color: #000000;">: </span><span style="color: #098658;">915</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"barcode"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"MM915"</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"external_key"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"VX-90"</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"umid"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; },</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"cust_id"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"company_name"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"master_desc"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"barcode"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"MM915"</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">...</span>
          </div>
        </div>
      </td>
    </tr>
  </tbody>
</table>
<p>
  <span class="wysiwyg-color-black60"><em>URL Parameter:- alternatekeyhandling:<strong>ignore&nbsp;</strong></em></span>
</p>
<table style="border-collapse: collapse; width: 100%;" border="1">
  <tbody>
    <tr>
      <td style="width: 100%;">
        <div>
          <div>
            <span style="color: #000000;">{</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; </span><span style="color: #0451a5;">"L"</span><span style="color: #000000;">: [</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; {</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"master_no"</span><span style="color: #000000;">: {</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"master_no"</span><span style="color: #000000;">: </span><span style="color: #098658;">915</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; },</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"cust_id"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"company_name"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"master_desc"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"barcode"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"MM915"</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">...</span>
          </div>
        </div>
      </td>
    </tr>
  </tbody>
</table>
<p>
  Notice how the additional key fields barcode, external_key &amp; umid are not
  provided.
</p>
<p>
  It's recommended to always include this header with the value 'ignore', unless
  working with alternate keys.
</p>
<h4 id="h_01HKSQ339VZTSRBSD25DCPTR8A">
  <strong><span class="wysiwyg-color-blue120">nullvaluehandling</span></strong>
</h4>
<p class="wysiwyg-text-align-justify">
  Suppresses all null value fields from the response. Using this parameter reduces
  the payload size dramatically, especially for larger queries (v10.6).
</p>
<p>
  Applicable to all GET calls for List, Maintenance &amp; Report endpoints.
</p>
<p>
  Values are ‘<strong>ignore</strong>’ or ‘<strong>include</strong>’.
</p>
<p>
  Default value = ‘include’ (includes null values in the response)
</p>
<p>Example:</p>
<p>
  <span class="wysiwyg-color-black60"><em>URL Parameter:- nullvaluehandling=<strong>include</strong> (default)&nbsp;</em></span>
</p>
<table style="border-collapse: collapse; width: 100%; height: 266px;" border="1">
  <tbody>
    <tr style="height: 266px;">
      <td style="width: 100%; height: 266px;">
        <div>
          <div>
            <span style="color: #000000;">{</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; </span><span style="color: #0451a5;">"L"</span><span style="color: #000000;">: [</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; {</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"master_no"</span><span style="color: #000000;">: {</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"master_no"</span><span style="color: #000000;">: </span><span style="color: #098658;">915</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"barcode"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"MM915"</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"external_key"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"VX-90"</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"umid"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; },</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"cust_id"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"company_name"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"master_desc"</span><span style="color: #000000;">: </span><span style="color: #0000ff;">null</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"barcode"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"MM915"</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">...</span>
          </div>
        </div>
      </td>
    </tr>
  </tbody>
</table>
<p>
  <span class="wysiwyg-color-black60"><em>URL Parameter:- nullvaluehandling=<strong>ignore</strong>&nbsp;</em></span>
</p>
<table style="border-collapse: collapse; width: 100%;" border="1">
  <tbody>
    <tr>
      <td style="width: 100%;">
        <div>
          <div>
            <span style="color: #000000;">{</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; </span><span style="color: #0451a5;">"L"</span><span style="color: #000000;">: [</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; {</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"master_no"</span><span style="color: #000000;">: {</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"master_no"</span><span style="color: #000000;">: </span><span style="color: #098658;">915</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"barcode"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"MM915"</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"external_key"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"VX-90"</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; },</span>
          </div>
          <div>
            <span style="color: #000000;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color: #0451a5;">"barcode"</span><span style="color: #000000;">: </span><span style="color: #a31515;">"MM915"</span><span style="color: #000000;">,</span>
          </div>
          <div>
            <span style="color: #000000;">...</span>
          </div>
        </div>
      </td>
    </tr>
  </tbody>
</table>
<p>
  Notice how all null value fields are omitted.<br>
  It's recommended to always include this parameter with the value 'ignore', unless
  visibility of null values is required.
</p>
<h4 id="h_01HF36DD3YKS0AAT7CYJT4MW00">
  <strong><span class="wysiwyg-color-blue120">Source-Time-Zone-Name</span></strong>
</h4>
<p>
  The REST API uses Date time formats in ISO format with an optional offset value.
</p>
<p class="wysiwyg-indent2">
  e.g. <span class="wysiwyg-color-cyan110">2014-11-03T22:20:00+00:00</span>
</p>
<p>
  If you omit the offset value when using POST to create a record, you can use
  a header parameter to set the time zone your dates are using.<br>
  <strong>Key: Source-Time-Zone-Name&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;Value: {Windows Time Zone name}</strong>
</p>
<p class="wysiwyg-indent2">
  e.g. : header '<span class="wysiwyg-color-blue70">Source-Time-Zone-Name: Pacific Standard Time '</span>
</p>
<p>
  Will create records using Pacific Standard Time as the time zone for the payload
  times.<br>
  Remember to omit the offset values in your time formats.
</p>
<h2 id="h_01HF36JJ5WR17BWBNHKZZCCQR1">
  <strong><span class="wysiwyg-color-blue120">PATCH Using POST Payload</span></strong>
</h2>
<p>
  There are two PATCH methods, one where you define the field and the operation,
  the other is to use the full POST JSON payload.
</p>
<p>
  To use the PATCH with the full JSON payload, you must include the header parameter:
</p>
<p>
  <strong>'Content-Type: <span class="wysiwyg-color-red110">application/json-patch+json</span>'</strong>
</p>
<p>Example Job update (cURL format):</p>
<p class="wysiwyg-indent3">curl --location --request PATCH</p>
<div class="wysiwyg-indent3">
  <div class="wysiwyg-indent3">
    <span class="wysiwyg-color-blue120">'https: //{base_url}/api/v2/database/{database_name}/JmJob/job_no=345' \</span>
  </div>
  <div class="wysiwyg-indent3">
    --header&nbsp;'Content-Type:&nbsp;application/json-patch+json'&nbsp;\
  </div>
  <div class="wysiwyg-indent3">
    --header&nbsp;'Authorization:&nbsp;Basic&nbsp;eHl0ZWNoOk1lZGlhMjwqNSE='&nbsp;\
  </div>
  <div class="wysiwyg-indent3">--data&nbsp;'{</div>
  <div class="wysiwyg-indent3">
    &nbsp;&nbsp;<span class="wysiwyg-color-red120">&nbsp;&nbsp;"jm_job"</span>:&nbsp;[
  </div>
  <div class="wysiwyg-indent3">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{</div>
  <div class="wysiwyg-indent3">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="wysiwyg-color-red120">&nbsp;"job_no"</span>:&nbsp;{
  </div>
  <div class="wysiwyg-indent3">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"job_no":<span class="wysiwyg-color-blue120">&nbsp;345</span>
  </div>
  <div class="wysiwyg-indent3">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},
  </div>
  <div class="wysiwyg-indent3">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"<span class="wysiwyg-color-red120">job_desc"</span>:<span class="wysiwyg-color-blue120">"Leaves&nbsp;of&nbsp;October",</span>
  </div>
  <div class="wysiwyg-indent3">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="wysiwyg-color-red120">"job_type_no"</span>:&nbsp;{
  </div>
  <div class="wysiwyg-indent3">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="wysiwyg-color-red120">"job_type_no</span>":&nbsp;7
  </div>
  <div class="wysiwyg-indent3">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},
  </div>
  <div class="wysiwyg-indent3">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="wysiwyg-color-red120">&nbsp;"po"</span>:&nbsp;<span class="wysiwyg-color-blue120">"PO1234",</span>
  </div>
  <div class="wysiwyg-indent3">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="wysiwyg-color-red120">&nbsp;"job_reference"</span>:
    <span class="wysiwyg-color-blue120">"Streets&nbsp;Ahead"</span>
  </div>
  <div class="wysiwyg-indent3">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}</div>
  <div class="wysiwyg-indent3">&nbsp;&nbsp;&nbsp;&nbsp;]</div>
  <div class="wysiwyg-indent3">}'</div>
</div>
<p>
  <strong>Notes:</strong>
</p>
<p>
  The payload must include the existing primary key (job_no in the above example)
  as a URL parameter as well as in the body payload.
</p>
<p>
  The expected response status code for a successful PATCH is ‘204’ and there will
  be no response body.
</p>
<h2 id="h_01HF40NRXV8NZWCRJ5PV9TMSZ1">
  <strong><span class="wysiwyg-color-blue120">PATCH using List endpoints</span></strong>
</h2>
<p>
  List endpoints also support the PATCH method so that you can update multiple
  records using a query (see ‘GET – Query Parameters’ section above for details
  on available query parameters). This method is teh equilivant API functionality
  of Grid Update feature of the UI.<br>
  Example to update multiple jobs using the $in query parameter:
</p>
<div>
  <div>
    <span class="wysiwyg-color-black"><strong>PATCH</strong>&nbsp;<em>{base_url}</em>/JmJobList?query={"job_no": {"$in": ["410","411"]}}</span>
  </div>
  <div class="wysiwyg-indent3">&nbsp;</div>
  <div class="wysiwyg-indent3">[</div>
  <div class="wysiwyg-indent3">&nbsp;&nbsp;&nbsp;&nbsp;{</div>
  <div class="wysiwyg-indent3">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="wysiwyg-color-red120">"op":</span>&nbsp;<span class="wysiwyg-color-blue120">"replace"</span>,
  </div>
  <div class="wysiwyg-indent3">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="wysiwyg-color-red120">"path"</span>:<span class="wysiwyg-color-blue120">&nbsp;"job_reference"</span>,
  </div>
  <div class="wysiwyg-indent3">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="wysiwyg-color-red120">&nbsp;"value"</span>:&nbsp;<span class="wysiwyg-color-blue120">"These&nbsp;are&nbsp;not&nbsp;taxed"</span>
  </div>
  <div class="wysiwyg-indent3">&nbsp;&nbsp;&nbsp;&nbsp;}</div>
  <div class="wysiwyg-indent3">]</div>
  <div>&nbsp;</div>
  <div>
    As&nbsp;with&nbsp;GET,&nbsp;wildcards&nbsp;‘%’&nbsp;are&nbsp;supported&nbsp;such&nbsp;as:
  </div>
  <div>
    <span class="wysiwyg-color-black"><strong>PATCH </strong></span><span class="wysiwyg-color-blue"><em>{base_url}</em><em>/</em>&nbsp;JmJobList?Query={"job_desc":&nbsp;"Sport%"}</span>
  </div>
</div>
<h2 id="h_01HF36G51KQBZK969TYVT0Y4BW">
  <strong><span class="wysiwyg-color-blue120">POST method</span></strong>
</h2>
<p>
  <span class="wysiwyg-color-black"><strong>Mandatory Fields to Create New Items</strong></span>
</p>
<p>
  The Swagger model identifies many fields as mandatory (nullable: false), which
  indicates that a value must be provided when the item is written to the database.
  However, in some cases, the business logic will provide default values, so it
  may not be strictly necessary to pass values for these fields to the API to create
  a new record.&nbsp;<br>
  For example, in a basic payload to create a Work Order, Swagger identifies at
  least 25 fields as mandatory, but the most basic payload to create a Work Order
  is below:
</p>
<table style="border-collapse: collapse; width: 100%;" border="1">
  <tbody>
    <tr>
      <td style="width: 100%;">
        <div class="wysiwyg-indent3">{</div>
        <div class="wysiwyg-indent3">
          &nbsp;&nbsp;&nbsp;<span class="wysiwyg-color-red120">&nbsp;"jm_work_order</span>":&nbsp;[
        </div>
        <div class="wysiwyg-indent3">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{</div>
        <div class="wysiwyg-indent3">
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="wysiwyg-color-red120">&nbsp;"external_key"</span>:&nbsp;<span class="wysiwyg-color-blue120">"externalID"</span>,
        </div>
        <div class="wysiwyg-indent3">
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="wysiwyg-color-red120">&nbsp;"wo_desc"</span>:<span class="wysiwyg-color-blue120">&nbsp;"Match2"</span>,
        </div>
        <div class="wysiwyg-indent3">
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="wysiwyg-color-red120">&nbsp;"wo_begin_dt"</span>:&nbsp;<span class="wysiwyg-color-blue120">"2022-01-01T00:00",</span>
        </div>
        <div class="wysiwyg-indent3">
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="wysiwyg-color-red120">"wo_end_dt"</span>:&nbsp;<span class="wysiwyg-color-blue120">"2022-01-01T06:00",</span>
        </div>
        <div class="wysiwyg-indent3">
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="wysiwyg-color-red120">&nbsp;&nbsp;&nbsp;"wo_type_no"</span>:&nbsp;<span class="wysiwyg-color-blue120">2,</span>
        </div>
        <div class="wysiwyg-indent3">
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="wysiwyg-color-red120">"phase_code"</span>:&nbsp;<span class="wysiwyg-color-blue120">"Bid"</span>,
        </div>
        <div class="wysiwyg-indent3">
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="wysiwyg-color-red120">"rate_card_no"</span>:&nbsp;<span class="wysiwyg-color-blue120">1,</span>
        </div>
        <div class="wysiwyg-indent3">
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="wysiwyg-color-red120">&nbsp;&nbsp;&nbsp;"cust_id":</span>&nbsp;<span class="wysiwyg-color-blue120">"6"</span>,
        </div>
        <div class="wysiwyg-indent8">
          &nbsp;<span class="wysiwyg-color-red120">"wo_template_no"</span>:&nbsp;<span class="wysiwyg-color-blue120">"2"</span>
        </div>
        <div class="wysiwyg-indent3">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}</div>
        <div class="wysiwyg-indent3">&nbsp;&nbsp;&nbsp;&nbsp;]</div>
        <div class="wysiwyg-indent3">}</div>
      </td>
    </tr>
  </tbody>
</table>
<p id="h_01HF36FFC3ZFAGYJPAG8FCM4DD">
  See other basic payload examples in the public
  <a href="/hc/en-us/articles/21789230412571#h_01HF433TKM2GMHVRE5CPR8X3X6">Postman Collection</a>.
</p>
<h2 id="h_01HKSSB696J0EHXSY0WNGE2RCQ">
  <span class="wysiwyg-color-blue120"><strong>Custom Field handling</strong></span>
</h2>
<p>
  <span class="wysiwyg-color-black">Any custom fields you have defined through document customization are automatically included in REST API payloads. The naming convention is a combination of the Custom Code you created plus the internal field name concatenated with an underscore. i.e. {customization code}_{field name}</span>
</p>
<p>
  <span class="wysiwyg-color-black70"><em>Document customization screen showing the Customnization Code</em></span>
</p>
<p>
  <img src="/guide-media/01HKT44WJKY92VA91WPASE8SD7">
</p>
<p>
  <span class="wysiwyg-color-black70"><em>Document customization screen showing the internal field names:</em></span>
</p>
<p>
  <img src="/guide-media/01HKT4CZ1EQPW8N6HN5WE7Y9K3" alt="2024-01-10_16-44-24.jpg">
</p>
<p>
  <span class="wysiwyg-color-black70"><em>Response from a GET call to fetch a Work Order showing the payload custom fields:</em></span>
</p>
<p>
  <img src="/guide-media/01HKT4JWNR3Q0CZ5S1JWZ17E59">
</p>
<h2 id="h_01HF436SPYJR54ZAYHZFVSCF3W">
  <strong><span class="wysiwyg-color-blue120">Appendix A – Endpoint List</span></strong>
</h2>
<p>
  There are over 1000 individual documents (endpoints) available to the Platform’s
  REST API. Below is a small sample of those documents. The full list of available
  documents can be obtained directly from the Platform using the Document Customizations
  query found in the System module. Specific documentation on each document can
  be found via the Swagger site.<br>
  <br>
</p>
<table style="border-collapse: collapse; width: 100.143%;" border="1">
  <tbody>
    <tr>
      <td style="width: 13.1429%;">
        <strong>ID&nbsp;</strong>
      </td>
      <td style="width: 27.2857%;">
        <strong>Class Name&nbsp;</strong>
      </td>
      <td style="width: 32.2857%;">
        <strong>Document Description&nbsp;</strong>
      </td>
      <td style="width: 27.4286%;">
        <strong>Document Type</strong>
      </td>
    </tr>
    <tr>
      <td style="width: 13.1429%;">10315&nbsp;</td>
      <td style="width: 27.2857%;">JmJob&nbsp;</td>
      <td style="width: 32.2857%;">Job&nbsp;</td>
      <td style="width: 27.4286%;">Maintenance</td>
    </tr>
    <tr>
      <td style="width: 13.1429%;">315&nbsp;</td>
      <td style="width: 27.2857%;">JmJobList&nbsp;</td>
      <td style="width: 32.2857%;">Jobs&nbsp;</td>
      <td style="width: 27.4286%;">Select (List)</td>
    </tr>
    <tr>
      <td style="width: 13.1429%;">10317&nbsp;</td>
      <td style="width: 27.2857%;">JmJobStatus&nbsp;</td>
      <td style="width: 32.2857%;">Job Statuses&nbsp;</td>
      <td style="width: 27.4286%;">Setup</td>
    </tr>
    <tr>
      <td style="width: 13.1429%;">10318&nbsp;</td>
      <td style="width: 27.2857%;">JmJobTable1&nbsp;</td>
      <td style="width: 32.2857%;">Subscription&nbsp;</td>
      <td style="width: 27.4286%;">Setup</td>
    </tr>
    <tr>
      <td style="width: 13.1429%;">10322&nbsp;</td>
      <td style="width: 27.2857%;">JmJobType&nbsp;</td>
      <td style="width: 32.2857%;">Job Types&nbsp;</td>
      <td style="width: 27.4286%;">Setup</td>
    </tr>
    <tr>
      <td style="width: 13.1429%;">359&nbsp;</td>
      <td style="width: 27.2857%;">JmTrxReport&nbsp;</td>
      <td style="width: 32.2857%;">Transaction Reports&nbsp;</td>
      <td style="width: 27.4286%;">Select (List)</td>
    </tr>
    <tr>
      <td style="width: 13.1429%;">10339&nbsp;</td>
      <td style="width: 27.2857%;">JmWorkOrder</td>
      <td style="width: 32.2857%;">Work Order&nbsp;</td>
      <td style="width: 27.4286%;">Maintenance</td>
    </tr>
    <tr>
      <td style="width: 13.1429%;">10346&nbsp;</td>
      <td style="width: 27.2857%;">JmWoTransaction&nbsp;</td>
      <td style="width: 32.2857%;">Work Order Transactions&nbsp;</td>
      <td style="width: 27.4286%;">Maintenance</td>
    </tr>
  </tbody>
</table>
<h4 id="h_01HF436SPY7179Z7YT86HJW6HF">&nbsp;</h4>
<h2 id="h_01HF436A57T9EYFPJB4NFHXTPH">
  <strong><span class="wysiwyg-color-blue120">Appendix B – Swagger Examples</span></strong>
</h2>
<p>
  <img src="/guide-media/01HF42NHHVPYWH2331XN5ZAMGM">
</p>
<p>
  <img src="/guide-media/01HF42QF0H65HERCBVNSF3TWSY">
</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>