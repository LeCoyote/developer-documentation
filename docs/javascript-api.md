# JavaScript API

Piwik is equipped with a powerful JavaScript Tracking API. Advanced users can use the Piwik tracking code to customize the way some of the web analytics data is recorded in Piwik. Plugin developers can also pass custom JSON data to the Piwik tracking script, and use this data in plugins.

## Where can I Find the Piwik Tracking Code?

To use all the features described in this page, you need to use the latest version of the tracking code. To find the tracking code for your website, please follow the steps below:

*   Log in to Piwik with your admin or Super User account
*   Click on Settings to access the administration area
*   Click on Websites to list the websites that you are tracking in Piwik
*   Click on View Tracking code for the website you wish to track
*   You can now copy and paste the JavaScript Tracking code into your pages, just before the &lt;/body&gt; tag.

The Piwik Tracking code looks as follows:

    &lt;!-- Piwik --&gt; &lt;script type="text/javascript"&gt;
    var _paq = _paq || [];
    (function(){ var u=(("https:" == document.location.protocol) ? "https://{$PIWIK_URL}/" : "http://{$PIWIK_URL}/");
    _paq.push(['setSiteId', {$IDSITE}]);
    _paq.push(['setTrackerUrl', u+'piwik.php']);
    _paq.push(['trackPageView']);
    _paq.push(['enableLinkTracking']);
    var d=document, g=d.createElement('script'), s=d.getElementsByTagName('script')[0]; g.type='text/javascript'; g.defer=true; g.async=true; g.src=u+'piwik.js';
    s.parentNode.insertBefore(g,s); })();
     &lt;/script&gt;
    &lt;!-- End Piwik Code --&gt;
    `</pre>

    In your Piwik tracking code, {$PIWIK_URL} would be replaced by your Piwik URL and {$IDSITE} would be replaced by the idsite of the website you are tracking in Piwik.

    This code might look a bit strange to those of you familiar with JavaScript, but that is because it is made to run asynchronously. In other words, browsers will not wait for the piwik.js file to be downloaded in order to show your page.

    For asynchronous tracking, configuration and tracking calls are pushed onto the global _paq array for execution, independent of the asynchronous loading of piwik.js. The format is:

    <pre>_paq.push([ 'API_method_name', parameter_list ]);</pre>

    You can also push functions to be executed. For example:

    <pre>`var visitor_id;
    _paq.push([ function() { visitor_id = this.getVisitorId(); }]);
    `</pre>

    or for example, to fetch a custom variable (name,value) using the asynchronous code:

    <pre>`
    _paq.push(['setCustomVariable','1','VisitorType','Member']);
    _paq.push([ function() { var customVariable = this.getCustomVariable(1); }]);
    `</pre>

    You can push to the _paq array even after the piwik.js file has been loaded and run.

    If your Piwik tracking code doesn’t look like this one, you may be using the deprecated version. Older versions still work as expected and will track your visitors, but we highly recommend that you update your pages to use the most recent tracking code.

    ## Features of the JavaScript Tracker

    ## Customize the Page Name Displayed in Piwik

    By default, Piwik uses the URL of the current page as the page title in the Piwik interface. If your URLs are not simple, or if you want to customize the way Piwik tracks your pages, you can specify the page title to use in the JavaScript code.

    A common use is to set the HTML Title value as the document title:

    `[...]
    **<span style="color: #ff0000;">_paq.push(['setDocumentTitle', document.title]);</span>**

    _paq.push(['trackPageView']);

    [...]
    `

    If you track **multiple sub-domains in the same website**, you may want your page titles to be prefixed by the sub-domain make it easy for you to see the traffic and data for each sub-domain. You can do so simply in JavaScript:

    `[...]
    <span style="color: #ff0000;">_paq.push(['setDocumentTitle', document.domain + "/" + document.title]);</span>

    _paq.push(['trackPageView']);

    [...]`

    Advanced users can also dynamically generate the page name, for example, in PHP:

    `[...]
    <span style="color: #ff0000;">_paq.push(['setDocumentTitle', "&lt;?php echo $myPageTitle ?&gt;"]);</span>

    _paq.push(['trackPageView']);

    [...]`

    ## Manually Trigger a Page View on Click or on JS Event

    By default, Piwik tracks page views when the Javascript tracking code loads and executes on each page view. However, on modern websites or web applications, user interactions do not necessarily involve loading a new page. For example, when users click on a JavaScript link, or when they click on a tab (which triggers a JS event), or when they interact with elements of the user interface, you can still track these interactions with Piwik.

    To track any user interaction or click with Piwik, you can manually call the Javascript function trackPageView(). For example, if you wanted to track a click on a JavaScript menu, you could write:

    `[...]
    &lt;a href="#" onclick="<span style="color: #ff0000;">javascript:_paq.push(['trackPageView', 'Menu/Freedom']);</span>"&gt;Freedom page&lt;/a&gt;

    `

    ## Manually Trigger a Conversion for a Goal

    By default, Goals in Piwik are defined as “matching” parts of the URL (starts with, contains, or regular expression matching). You can also track goals for given page views, downloads, or outlink clicks.

    In some situations, you may want to register conversions on other types of actions, for example:

*   when a user submits a form
*   when a user has stayed more than a given amount of time on the page
*   when a user does some interaction in your Flash application
*   when a user has submitted his cart and has done the payment: you can give the Piwik tracking code to the payment website which will then register the conversions in your Piwik database, with the conversion’s custom revenue

    To trigger a goal using the Piwik JavaScript Tracking, you can simply do:

    `[...]
    **<span style="color: #ff0000;">_paq.push(['trackGoal', 1]); // logs a conversion for goal 1</span>**

    [...]
    `

    You can also register a conversion for this goal with a custom revenue. For example, you can generate the call to trackGoal dynamically to set the revenue of the transaction:

    `[...]**<span style="color: #ff0000;">

    // logs a conversion for goal 1 with the custom revenue set

    _paq.push(['trackGoal', 1, &lt;?php echo $cart-&gt;getCartValue(); ?&gt;]);</span>**

    [...]`

    Find more information about goal tracking in Piwik in the [**Tracking Goals**](http://piwik.org/docs/tracking-goals-web-analytics/) documentation.

    ## Tracking Ecommerce Orders, Cart Updates and Product/Category Page Views

    Piwik allows for advanced and powerful Ecommerce tracking. Check out the [Ecommerce Analytics](http://piwik.org/docs/ecommerce-analytics/) documentation for more information about Ecommerce reports and how to set up Ecommerce tracking.

    ## Tracking internal Search Keywords, categories, and No Result Search Keywords

    Piwik offers advanced [Site Search Analytics](http://piwik.org/docs/site-search/) feature, letting you track how your visitors use your internal website search engine. By default, Piwik can read URL parameters that will contain the search keyword. However, you can also record the site search keyword manually using the Javascript function _trackSiteSearch(…)_

    In your website, in standard pages, you would typically have a call to record Page views via piwikTracker.trackPageView(). On your search result page, you would call **instead** piwikTracker.trackSiteSearch(keyword, category, searchCount) function to record the internal search request. Note: the ‘keyword’ parameter is required, but category and searchCount are optional.

    <pre>`[...]
    <span style="color: #ff0000;">_paq.push(['trackSiteSearch',
    "Banana", // Search keyword searched for
    "Organic Food", // Search category selected in your search engine. If you do not need this, set to false
    0 // Number of results on the Search results page. Zero indicates a 'No Result Search Keyword'. Set to false if you don't know
    ]);</span>
    // We recommend not to call trackPageView() on the Site Search Result page
    // _paq.push(['trackPageView']);
    [...]
    `</pre>

    We also highly recommend to set the searchCount parameter, as Piwik will specifically report “No Result Keywords”, ie. Keywords that were searched, but did not return any result. It is usually very interesting to know what users search for but can’t find (yet?) on your website. Learn more about [Site Search Analytics in the User Doc](http://piwik.org/docs/site-search/).

    ## Custom Variables

    Custom variables are a powerful feature that enable you to track custom values for each visit, and/or each page view. Please see the [Tracking custom variables](http://piwik.org/docs/custom-variables/) documentation page for general information.

    You can se tup up to 5 custom variables (name and value) for each visit to your website, and/or up to 5 custom variables for each page view. If you set a custom variable to a visitor, when he comes back 1 hour or 2 days later, it will be a new visit and his/her custom variables will be empty.

    There are two “scopes” which you can set your custom variables to. The “scope” is the 4th parameter of the function setCustomVariable().

*   when scope = “visit”, the custom variable’s name and value will be stored in the visit in the database. You can therefore store up to 5 custom variables of scope “visit” for each visit.
*   when scope = “page”, the custom variable’s name and value will be stored for the page view being tracked. You can therefore store up to 5 custom variables of scope “page” for each page view.

    Custom variable statistics are reported in Piwik under **Visitors &gt; custom variables**. Both custom variables of scope “visit” and “page” are aggregated in this report.

    **Set a Custom Variable for the Visit**

    <pre>**setCustomVariable (index, name, value, scope = "visit")**</pre>

    This function is used to create, or update a custom variable name and value. For example, imagine you want to store in each visit the gender of the user. Yow would store the custom variable with a name = “gender”, value = “male” or “female”.

    **<span style="color: #ff6600;">Important:</span>** a given custom variable name must always be stored in the same “index”. For example, if you choose to store the variable **name = “Gender”** in **index = 1** and you record another custom variable in index = 1, then the “Gender” variable will be deleted and replaced with the new custom variable stored in index 1.

    <pre>`[...]
    <span style="color: #ff0000;">_paq.push(['setCustomVariable',
    1, // Index, the number from 1 to 5 where this custom variable name is stored
    "Gender", // Name, the name of the variable, for example: Gender, VisitorType
    "Male", // Value, for example: "Male", "Female" or "new", "engaged", "customer"
    "visit" // Scope of the custom variable, "visit" means the custom variable applies to the current visit
    ]);</span>
    _paq.push(['trackPageView']);
    [...]
    `</pre>

    You only need to set a variable with scope “visit” once, and the value will be recorded for the whole visit.

    **Set a Custom Variable for a Page View**

    <pre>**setCustomVariable (index, name, value, scope = "page")**</pre>

    As well as tracking custom variables for “visits”, it is sometimes useful to track custom variables for each page view separately. For example, for a “News” website or blog, a given article may be categorized into one or several categories. In this case, you could set one or several custom variables with name=”category”, one with value=”Sports” and another with value=”Europe” if the article is classified in Sports and Europe Categories. The custom variables report will then report on how many visits and page views were in each of your website’s categories. This information can be difficult to obtain with standard Piwik reports because they report on “Best Page URLs” and “Best Page Titles” which might not contain the “category” information.

    <pre>`[...]
    <span style="color: #ff0000;">// Track 2 custom variables with the same name, but in different slots.
    // You will then access the statistics about your articles' categories in the 'Visitors &gt; custom variables' report
    _paq.push(['setCustomVariable', 1, 'Category', 'Sports', 'page']);
    _paq.push(['setCustomVariable', 2, 'Category', 'Europe', 'page']); // Track the same name but in a different Index
    // Here you could track other custom variables with scope "page" in Index 3, 4 or 5
    // The order is important: first setCustomVariable is called and then trackPageView records the request
    </span>_paq.push(['trackPageView']);
    [...]
    `</pre>

    **<span style="color: #ff6600;">Important:</span>** It is possible to store a custom variables of scope “visit” in “index” 1, and store a different custom variable of scope “page” in the same “index” 1. Therefore, you can technically **track up to 10 custom variables names and values on every page** of your website (5 with a “page” scope stored in the actual page view, 5 with a “visit” scope stored in the visit).

    <pre>`[...]
    <span style="color: #ff0000;">_paq.push(['setCustomVariable',
    1, // Index, the number from 1 to 5 where this custom variable name is stored for the current page view
    "category", // Name, the name of the variable, for example: Category, Sub-category, UserType
    "Sports", // Value, for example: "Sports", "News", "World", "Business", etc.
    "page" // Scope of the custom variable, "page" means the custom variable applies to the current page view
    ]);</span>
    _paq.push(['trackPageView']);
    [...]
    `</pre>

    **Delete a Custom Variable**

    <pre>**deleteCustomVariable (index, scope )**</pre>

    If you created a custom variable and then decide to remove this variable from a visit or page view, you can use deleteCustomVariable.

    To persist the change in the Piwik server, you must call the function before the call to trackPageView();

    `[...]**

    **<span style="color: #ff0000;">_paq.push(['deleteCustomVariable', 1, "visit"]); // Delete the variable in index 1 stored for the current visit</span>

    _paq.push(['trackPageView']);

    [...]`

    **Get the Name and Value of a Custom Variable**

    <pre>**getCustomVariable (index, scope )**</pre>

    This function is mostly useful if scope = “visit”.

    In this case, custom variables are recorded in a first party cookie for the duration of the visit (30 minutes after the last action). You can actually retrieve the custom variable names and values using piwikTracker.getCustomVariable. If there is no custom variable in the requested index, it will return false.

    `[...]**

    **<span style="color: #ff0000;">_paq.push([ function() {

        var customVariable = this.getCustomVariable( 1, "visit" ); // Returns the custom variable: [ "gender", "male" ]

    // do something with customVariable...

    }]);</span>

    _paq.push(['trackPageView']);

    [...]`

    ## Cookie Configuration for Domains and Subdomains

    Piwik uses first-party cookies to keep track some information (number of visits, original referrer, and unique visitor ID). First[party cookies ensure higher user privacy (since cookies are not sent to a third-party server), and are therefore accepted in most browsers by default.

    Piwik creates a set of cookies for each domain and subdomain. If you want to track some subdomains and share the same cookie for accurate statistics, it is necessary to customize the Piwik Tracking code. Check out the examples for the various configurations.

    ### If you Track Only One Domain Name or Subdomain for a Single Website in Piwik

    This is the standard use case. Piwik tracks the visits of one domain name with no subdomain, in a single Piwik website.

    `[...]
    // Default Tracking code

    _paq.push(['setSiteId', 1]);

    _paq.push(['setTrackerUrl', u+'piwik.php']);

    _paq.push(['trackPageView']);

    [...]`

    ### If you Track One Domain Name and Several Subdomains for a Single Website in Piwik

    If you want to record visits for the main domain name as well as its subdomains, you would want to share cookies across all domains. You can do so by calling setCookieDomain(), in all subdomains tracking codes.

    `[...]
    _paq.push(['setSiteId', 1]);

    _paq.push(['setTrackerUrl', u+'piwik.php']);

    <span style="color: red;">_paq.push(['setCookieDomain', '*.example.com']); // Same cookie as: example.com, www.example.com, subdomain.example.com, ...</span>

    _paq.push(['setDomains', '*.example.com']); // Download &amp; Click tracking alias domains

    _paq.push(['trackPageView']);

    [...]`

    ### If you Track Domain Subdirectories or Pages in Different Websites in Piwik

    By default, Piwik uses only one cookie for a domain name, and all its pages and subdirectories.

    There may be cases where you track a subdirectory as a separate website in Piwik. If a visitor visits more than a few subdirectories, this will cause some inaccuracy in your reports: time on site, number of visits, conversion referrer, returning and new visitors. In this use case, you can ensure your reports stay accurate by creating a different cookie for each subpath you are tracking in different Piwik websites. The function setCookiePath() is used to set the Cookie path.

    For example, if your website has user profiles, you could track each user profile page analytics as a unique website in Piwik. In the main domain homepage, you would use the default tracking code.

    `[...]
    _paq.push(['setSiteId', X]); // idSite = X for the Homepage

    _paq.push(['setTrackerUrl', u+'piwik.php']);

    _paq.push(['trackPageView']);

    [...]`

    In the /user/MyUsername page, you would write:

    `[...]
    // The idSite Y will be different from other user pages

    _paq.push(['setSiteId', Y]);

    _paq.push(['setTrackerUrl', u+'piwik.php']);

    <span style="color: red;">_paq.push(['setCookiePath', '/user/MyUsername']);</span>

    _paq.push(['trackPageView']);

    [...]`

    For more information about tracking websites and subdomains in Piwik, see the FAQ: [How to configure Piwik to monitor several websites, domains and sub-domains](http://piwik.org/faq/new-to-piwik/#faq_104)

    ## Ignore Specific Domains or Subdomains in the "Outlink" Click Tracking

    By default all links to domains other than the current domain have click tracking enabled, and each click will be counted as an outlink. If you use multiple domains and subdomains, you may see clicks on your subdomains appearing in the Pages &gt; Outlinks report.

    If you only want clicks to external websites to appear in your outlinks report, you can use the function setDomains() to specify the list of alias domains or subdomains. Wildcard domains (*.example.org) are supported to let you easily ignore clicks to all subdomains.

    `[...]
    <span style="color: #ff0000;">// Don't track Outlinks on all clicks pointing to *.hostname1.com or *.hostname2.com

    // Note: the currently tracked website is added to this array automatically

    _paq(['setDomains', ["*.hostname1.com", "hostname2.com"]]); </span>

    _paq.push(['trackPageView']);

    [...]`

    ## Disable the Download &amp; Outlink Tracking

    By default, the Piwik tracking code enables clicks and download tracking. To disable all automatic download and outlink tracking, you must remove the call to the enableLinkTracking() function:

    `[...]
    <span style="color: #ff0000;">// we comment out the function that enables link tracking

    // _paq.push(['enableLinkTracking']);</span>

    _paq.push(['trackPageView']);

    [...]
    `

    ## Disable the Download &amp; Outlink Tracking for Specific CSS Classes

    You can disable automatic download and outlink tracking for links with specific CSS classes:

    `[...]
    <span style="color: #ff0000;">_paq.push(['setIgnoreClasses', "no-tracking"]); // you can also pass an array of strings</span>

    _paq.push(['trackPageView']);

    [...]`

    This will result in clicks on a link `&lt;a href='http://example.com' class='no-tracking'&gt;Test&lt;/a&gt;` not being counted.

    ## Disable Download or Outlink Tracking on a Specific Link

    If you want to always ignore download or outlink tracking on a specific link, you can add the 'piwik_ignore' css class to it:

    `&lt;a href='http://builds.piwik.org/latest.zip' <span style="color: #ff0000;">class='piwik_ignore'</span>&gt;File I don't want to track as a download&lt;/a&gt;`

    ## Force a Click on a Link to be Recorded as a Download in Piwik

    If you want Piwik to consider a given link as a download, you can add the 'piwik_download' css class to the link:

    `&lt;a href='last.php' <span style="color: #ff0000;">class='piwik_download'</span>&gt;Link I want to track as a download&lt;/a&gt;`

    Note: you can customize and rename the CSS class used to force a click to be recorded as a download:

    `[...]
    <span style="color: #ff0000;">// now all clicks on links with the css class "download" will be counted as downloads</span>

    <span style="color: #ff0000;">_paq.push(['setDownloadClasses', "download"]); // you can also pass an array of strings</span>

    _paq.push(['trackPageView']);

    [...]`

    ## Force a Click on a Link to be Recorded as an Outlink

    If you want Piwik to consider a given link as an outlink (links to the current domain or to one of the alias domains), you can add the 'piwik_link' css class to the link:

    `&lt;a href='http://mysite.com/partner/' <span style="color: #ff0000;">class='piwik_link'</span>&gt;Link I want to track as an outlink&lt;/a&gt;`

    Note: you can customize and rename the CSS class used to force a click to being recorded as an outlink:

    `[...]
    <span style="color: #ff0000;">// now all clicks on links with the css class "external" will be counted as outlinks</span>

    <span style="color: #ff0000;">_paq.push(['setLinkClasses', "external"]); // you can also pass an array of strings</span>

    _paq.push(['trackPageView']);

    [...]`

    Alternatively, you can use JavaScript to manually trigger a click on an outlink (it will work the same for page views or file downloads). In this example, custom outlink is trigged when the email address is clicked:

    `&lt;a href="mailto:namexyz@mydomain.co.uk" target="_blank" onClick="javascript:_paq.push(['trackLink', 'http://mydomain.co.uk/mailto/Agent namexyz', 'link']);"&gt; namexyz@mydomain.co.uk &lt;/a&gt;`

    ## Changing the Pause Timer

    When a user clicks to download a file, or clicks on an outbound link, Piwik records it. In order to do so, it adds a small delay before the user is redirected to the requested file or link. The default value is 500ms, but you can set it to a shorter length of time. It should be noted, however, that doing so results in the risk that this period of time is not long enough for the data to be recorded in Piwik.

    `[...]
    <span style="color: #ff0000;">_paq.push(['setLinkTrackingTimer', 250]); // 250 milliseconds </span>

    _paq.push(['trackPageView']);

    [...]`

    ## Changing the List of File Extensions to Track as "Downloads"

    By default, any file ending with one of these extensions will be considered a 'download' in the Piwik interface:

    `7z|aac|arc|arj|asf|asx|avi|bin|bz|bz2|csv|deb|dmg|doc|

    exe|flv|gif|gz|gzip|hqx|jar|jpg|jpeg|js|mp2|mp3|mp4|mpg|

    mpeg|mov|movie|msi|msp|odb|odf|odg|odp|ods|odt|ogg|ogv|

    pdf|phps|png|ppt|qt|qtm|ra|ram|rar|rpm|sea|sit|tar|

    tbz|tbz2|tgz|torrent|txt|wav|wma|wmv|wpd||xls|xml|z|zip`

    To replace the list of extensions you want to track as file downloads, you can use setDownloadExtensions( string ):

    `[...]
    <span style="color: #ff0000;"> _paq.push(['setDownloadExtensions', "jpg|png|gif"]); // we now only track clicks on images</span>

    _paq.push(['trackPageView']);

    [...]`

    If you want to track a new filetype, you can just add it to the list by using addDownloadExtensions( filetype ):

    `[...]
    <span style="color: #ff0000;">// clicks on URLs finishing by mp5 or mp6 will be counted as downloads

    _paq.push(['addDownloadExtensions', "mp5|mp6"]); </span>

    _paq.push(['trackPageView']);

    [...]`

    ## Multiple Piwik Trackers

    It is possible to track a page using multiple Piwik trackers that point to the same or different Piwik servers. To improve page loading time, you can load piwik.js once. Each call to Piwik.getTracker() returns a unique Piwik Tracker object (instance) which can be configured.

    Note: When using multiple trackers, you must use the synchronous Javascript tracker object (ie. this will not work with the asynchronous tag):

    <pre>`&lt;script type="text/javascript"&gt;
    try {
    var piwikTracker = Piwik.getTracker("http://URL_1/piwik.php", 1);
    piwikTracker.trackPageView();
    var piwik2 = Piwik.getTracker("http://URL_2/piwik.php", 4);
    piwik2.trackPageView();
    } catch( err ) {}
    &lt;/script&gt;

Note that you can also set the website ID and the Piwik tracker URL manually, instead of setting them in the getTracker call:

`// we replace Piwik.getTracker("http://example.com/piwik/", 12)

var piwikTracker = Piwik.getTracker();

piwikTracker.setSiteId( 12 );

piwikTracker.setTrackerUrl( "http://example.com/piwik/" );

piwikTracker.trackPageView();`

## List of all Methods Available in the Tracking API

_Requesting the Tracker Instance from the Piwik Class_

*   <span class="function">Piwik.getTracker( trackerUrl, siteId ) </span>- get a new instance of the Tracker

    *   [Google Analytics equivalent] _getTracker(account)
    *   [Yahoo! Analytics equivalent] getTracker(account)

*   <span class="function">Piwik.getAsyncTracker() </span> - get the internal instance of the Tracker used for asynchronous tracking

_Using the Tracker Object_

*   <span class="function">enableLinkTracking( enable )</span> - Install link tracking on all applicable link elements. Set the enable parameter to true to use a pseudo-click handler to track browsers (such as Firefox) which don't generate click events for the middle mouse button. By default only "true" mouse click events are handled.
*   <span class="function">addListener( element )</span>- Add click listener to a specific link element. When clicked, Piwik will log the click automatically.
*   <span class="function">setRequestMethod( method )</span>- Set the request method to either "GET" or "POST". (The default is "GET".) To use the POST request method, the Piwik host must be the same as the tracked website host (Piwik installed in the same domain as your tracked website).
*   <span class="function">trackGoal( idGoal, [customRevenue]);</span> - Manually log a conversion for the goal idGoal, passing in the custom revenue customRevenue if specified
*   <span class="function">trackLink( url, linkType )</span>- Manually log a click from your own code. url is the full URL which is to be tracked as a click. linkType can either be 'link' for an outlink or 'download' for a download.
*   <span class="function">trackPageView([customTitle])</span>- Log visit to this page

    *   [Google Analytics equivalent] _trackPageview(opt_pageURL)
    *   [Yahoo! Analytics equivalent] submit()
    *   <span class="function">trackSiteSearch(keyword, [category], [resultsCount])</span>- Log an internal site search for a specific keyword, in an optional category, specifying the optional count of search results in the page.

_Configuration of the Tracker Object_

*   <span class="function">setDocumentTitle( string )</span>- Override document.title

    *   [Yahoo! Analytics equivalent] YWATracker.setDocumentName("xxx")

*   <span class="function">setDomains( array) </span>- Set array of hostnames or domains to be treated as local. For wildcard subdomains, you can use:
<pre>setDomains('.example.com');</pre>

or

    <pre>setDomains('*.example.com');</pre>

    *   [Google Analytics equivalent] _setDomainName(".example.com")
    *   [Yahoo! Analytics equivalent] setDomains("*.abc.net")

*   <span class="function">setCustomUrl( string ) </span> - Override the page's reported URL
*   <span class="function">setReferrerUrl( string ) </span> - Override the detected Http-Referer
*   <span class="function">setSiteId( integer )</span> - Specify the website ID. Redundant: can be specified in getTracker() constructor.
*   <span class="function">setTrackerUrl( string )</span> - Specify the Piwik server URL. Redundant: can be specified in getTracker() constructor.
*   <span class="function">setDownloadClasses( string | array )</span> - Set classes to be treated as downloads (in addition to piwik_download)
*   <span class="function">setDownloadExtensions( string )</span> - Set list of file extensions to be recognized as downloads. Example: 'doc|pdf|txt'
*   <span class="function">addDownloadExtensions( string )</span> - Specify additional file extensions to be recognized as downloads. Example: 'doc|pdf|txt'
*   <span class="function">setIgnoreClasses( string | array )</span> - Set classes to be ignored if

    present in link (in addition to piwik_ignore)
*   <span class="function">setLinkClasses( string | array )</span> - Set classes to be treated as

    outlinks (in addition to piwik_link)
*   <span class="function">setLinkTrackingTimer( integer )</span> - Set delay for link tracking in milliseconds.
*   <span class="function">discardHashTag( bool )</span> - Set to true to not record the hash tag (anchor) portion of URLs
*   <span class="function">setGenerationTimeMs(generationTime)</span> - By default Piwik uses the browser DOM Timing API to accurately determine the time it takes to generate and download the page. You may overwrite the value by specifying a milliseconds value here.
*   <span class="function">appendToTrackingUrl(appendToUrl)</span> - Appends a custom string to the end of the HTTP request to piwik.php?
*   <span class="function">setDoNotTrack( bool )</span> - Set to true to not track users who opt out of tracking using Mozilla's (proposed) Do Not Track setting.
*   <span class="function">disableCookies()</span> - Disables all first party cookies. Existing Piwik cookies for this websites will be deleted on the next page view.
*   <span class="function">deleteCookies()</span> - Deletes the tracking cookies currently currently set (this is useful when [creating new visits](http://piwik.org/faq/how-to/#faq_187))
*   <span class="function">killFrame()</span> - Enables a frame-buster to prevent the tracked web page from being framed/iframed.
*   <span class="function">redirectFile( url )</span> - Forces the browser load the live URL if the tracked web page is loaded from a local file (e.g., saved to someone's desktop).
*   <span class="function">setHeartBeatTimer( minimumVisitLength, heartBeatDelay )</span> - records how long the page has been viewed if the minimumVisitLength (in seconds) is attained; the heartBeatDelay determines how frequently to update the server
*   <span class="function">getVisitorId()</span> - returns the 16 characters ID for the visitor
*   <span class="function">getVisitorInfo()</span> - returns the visitor cookie contents in an array
*   <span class="function">getAttributionInfo()</span> - returns the visitor attribution array (Referer information and / or Campaign name &amp; keyword).

    Attribution information is by Piwik to credit the correct referrer ([first or last referrer](http://piwik.org/faq/general/#faq_106)) to any goal conversion.

    You can also use any of the following functions to get specific attributes of data:

    *   piwikTracker.getAttributionCampaignName()
    *   piwikTracker.getAttributionCampaignKeyword()
    *   piwikTracker.getAttributionReferrerTimestamp()
    *   piwikTracker.getAttributionReferrerUrl()

*   <span class="function">setCustomVariable (index, name, value, scope)</span> - Set a custom variable.
*   <span class="function">deleteCustomVariable (index, scope )</span> - Delete a custom variable.
*   <span class="function">getCustomVariable (index, scope )</span> - Retrieve a custom variable.
*   <span class="function">setCampaignNameKey(name)</span> - Set campaign name parameter(s). (Help: [Customize Campaign name parameter names](http://piwik.org/faq/how-to/#faq_120))
*   <span class="function">setCampaignKeywordKey(keyword)</span> - Set campaign keyword parameter(s). (Help: [Customize Campaign keyword parameter names](http://piwik.org/faq/how-to/#faq_120))
*   <span class="function">setConversionAttributionFirstReferrer( bool )</span> - Set to true to attribute a conversion to the first referrer. By default, conversion is attributed to the most recent referrer.

_Configuration of Tracking Cookies_

Starting with Piwik 1.2, first party cookies are used. Consideration must be given to retention times and avoiding conflicts with other cookies, trackers, and apps.

*   <span class="function">setCookieNamePrefix( prefix )</span> - the default prefix is '_pk_'.
*   <span class="function">setCookieDomain( domain )</span>- the default is the document domain; if your web site can be visited at both www.example.com and example.com, you would use:
<pre>tracker.setCookieDomain('.example.com');</pre>

or

    <pre>tracker.setCookieDomain('*.example.com');</pre>

*   <span class="function">setCookiePath( path )</span> - the default is '/'.
*   <span class="function">setVisitorCookieTimeout( seconds ) - the default is 2 years</span>
*   <span class="function">setSessionCookieTimeout( seconds ) - the default is 30 minutes</span>
*   <span class="function">setReferralCookieTimeout( seconds ) - the default is 6 months</span>

## Unit Tests Covering piwik.js

The Piwik JavaScript Tracking API is covered by an extensive JavaScript unit test suite to ensure that the code quality is as high as possible, and that we never break this functionality. Tests are written using QUnit. To run the tests, simply checkout the [Piwik Git repository](http://piwik.org/participate/contributing-with-git/) and go to /path/to/piwik/tests/javascript/. Tests are run inside your browser.

The Piwik JavaScript API has been tested with numerous web browsers. To maximize coverage, we use services like [crossbrowsertesting.com](http://crossbrowsertesting.com/) and [browsershots.org](http://browsershots.org/).

## Minify piwik.js

The piwik.js is minified to reduce the size that your website visitors will have to download. The YUI Compressor is used to minify the JavaScript ([more information](https://github.com/piwik/piwik/blob/master/js/README#L1)). You can find the original non minified version in [/js/piwik.js](https://github.com/piwik/piwik/blob/master/js/piwik.js#L1).

Questions?

If you have any question about JavaScript Tracking in Piwik, [please search the website](http://piwik.org/search/), or [ask in the forums](http://forum.piwik.org). Enjoy!

## Related documentation

*   [Does Piwik track visitors without JavaScript?](http://piwik.org/faq/new-to-piwik/#faq_63)
*   [How does Piwik track downloads?](http://piwik.org/faq/new-to-piwik/#faq_47)
*   [How to track error pages (404 pages) in Piwik? Which URLs are 404 and which referers lead to these pages?](http://piwik.org/faq/how-to/#faq_60)
*   [How can I set custom groups of pages (structure) so that page view are aggregated by categories?](http://piwik.org/faq/how-to/#faq_62)
*   [Is the Piwik JavaScript Tracking Code XHTML 1.0 compatible?](http://piwik.org/faq/general/#faq_66)
*   [Piwik shows "No Data" in all reports.](http://piwik.org/faq/troubleshooting/#faq_58)
*   [Visits from search engines or campaigns are not recorded, Piwik shows "No data available".](http://piwik.org/faq/troubleshooting/#faq_51)
*   [Statistics from Piwik and my other web analytics tool (log analyzer or javaScript based) are different, why?](http://piwik.org/faq/troubleshooting/#faq_50)
*   [The Piwik JavaScript code shows a red cross (on IE) or a broken image icon instead of the 1*1 transparent Pixel, what is the issue?](http://piwik.org/faq/troubleshooting/#faq_57)
*   [How do I disable all tracking cookies used by Piwik in the javascript code?](http://piwik.org/faq/general/#faq_157)