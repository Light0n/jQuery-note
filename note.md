# jQuery

```JavaScript
//selector and filter
$("p:first").css("border", "3px solid red");
$("h2:not(.selectors)").css("border", "3px solid red");
$("#idName p:first").css("border", "3px solid red");
$("#idName p:not(p:eq(2))").css("border", "3px solid red");
$("div > p").css("border", "3px solid red");//select all p that have direct parent as a div 
$("div p").css("border", "3px solid red");//select all p that are inside a div 
$("ul + div").css("border", "3px solid red");//select all div if they are right next to a ul element
$("#para1 ~ p").css("border", "3px solid red");//select all p that have element id para1 as their previous sibling
$("p[class]").css("border", "3px solid red");//select all p that have class attribute
$("p[class=a]").css("border", "3px solid red");//select all p that have class attribute value is "a"
$("p[id^=para]").css("border", "3px solid red");//select all p that have id attribute start with "para"
$("p[id^=para][lang*=en-]").css("border", "3px solid red");//select all p that have id attribute starts with "para" and lang attribute contains "en-"
$("p:contains('3')").css("border", "3px solid red");//select all p that have textContent contains string '3'
$("div:has(p[class=a])").css("border", "3px solid red");//select all div that has at least a p element with class attribute value is "a"
$("p:parent").css("border", "3px solid red");//select all p that has child node
$("div p:first-child").css("border", "3px solid red");//select all p that is the first-child of a div element
$("div p:last-of-type").css("border", "3px solid red");
//select all p that is the last p element of a div element
$("div p:nth-child(3)").css("border", "3px solid red");//select the 3rd p element which is inside a div
$("div p:nth-child(2n+0)").css("border", "3px solid red");//select the even (2,4,6 ...) p elements which are inside a div; formula An+B
$("div p:nth-child(-n+3)").css("border", "3px solid red");//select the first 3 p elements which are inside a div

//The :first pseudo-class is equivalent to :eq(0). It could also be written as :lt(1). While this matches only a single element, :first-child can match more than one: One for each parent.

//create new content and add to page
var newP = $("<p>");
newP.append("<em>Hello There</em>");
$("#example").html(newP);
//change the content directly
$("#example").html("<h2>This is a new H2</h2>");
$("#example").text("<h2>This is a new H2</h2>");//all html tags are plain texts

//insert content before old content
$("#creation").prepend("Watch This! ");

//event handle
$("document").ready(function() {
    $("#example").on("mousemove", onMouseOver);// add event
    $("#example").on("click", onMouseClick);// add event
});

function onMouseOver(evt) {
    $("#example").text(evt.type + ": " + evt.pageX + ", " + evt.pageY + "\n" +
                        "Button: " + evt.which + " Key: " + evt.metaKey);
}
function onMouseClick(evt) {
    $("#example").text(evt.type + ": " + evt.pageX + ", " + evt.pageY);
    $("#example").off("mousemove", onMouseOver);// remove event
}

//animation
    $("document").ready(function() {
    $("#go").click(function() {
        $("#testDiv").animate({width: 400}, 300)
        .animate({height: 300}, 400)
        .animate({left: 200}, 500)
        .animate({top: "+=100", borderWidth: 10}, "slow")//this is a chain of animations
    });
});

//ajax
$("document").ready(function() {
    $("#getcontent").click(getContent);
    $("#loadhtml").click(loadHTML);
});

function getContent() {
    $.ajax("sampletextcontent.txt",//file url
            { success: setContent, 
                type: "GET", 
                dataType: "text" });
}

function setContent(data, status, jqxhr) {
    $("#example").text(data);
}

function loadHTML() {
    $("#example").load("samplehtml.html");//file url
}
```

## Selectors

Syntax|Meaning
---|---
`$("tagName")`|All elements have **tagName**
`$("#idName")`|The element has id of **idName**
`$(".className")`|All elements have **className**
`$("tag.className")`|All **tag** elements have class **className**
`$("tag#id.className")`|The **tag** element has id of **id** and class **className**
`$(*)`|All elements in the page

## Filters
Syntax|Meaning
---|---
`:first`, `:last`|**first** or **last** of the matched set
`:even`, `:odd`|**even** or **odd** in the matched set
`:gt()`, `:lt()`, `:eq()`|Items greater than, less than, or equal to an index
`:animated`|Items that are in the process of being animated
`:focus`|The element that currently has the focus
`:not(expression)`|Elements that don't match the given expression 

## Tranversing

![jQuery Tranversing DOM](/img/Traversing&#32;DOM.png)

```JavaScript
$("#example").children().css("border", "3px solid red");//select all direct children of element has id "example" 

$("#example").find("#para4").css("border", "3px solid red");//find element has id "para4" inside element has id "example"

var leftmargin = 0;
var border = 3;

$("#example p").each(function(index, element){
    $(element).css("border", border+"px solid red").css("margin-left", leftmargin);//$(element): convert to jQuery object
    border +=2;
    leftmargin +=10;
});
```

## Manipulating Page Content

* Creating content
```JavaScript
$("#example").html("<p>The new content</p>");//change whatever in element that has id  "example"
$("#example p").text("<p>All HTML tags are considered as plain texts</p>");//this change textContent inside all p tags of example element
```
* Inserting content
```JavaScript
$("p").append("This content is added at the end of content inside p tags");
$("This content is added at the end of content inside p tags").appendTo("p");

$("p").prepend("This content is added at the start of content inside p tags");
$("This content is added at the start of content inside p tags").prependTo("p");

$("p").before("<p>This p element is added before p tags</p>");
$("<p>This p element is added before p tags</p>").insertBefore("p");

$("p").after("<p>This p element is added after p tags</p>");
$("<p>This p element is added after p tags</p>").insertAfter("p");
```

* Altering content
```JavaScript
$("#example p").wrap("<div style='color:red'>");//wrap each p inside example with a div
$("#example p").wrapAll("<div style='border:3px solid red'/>");//wrap all p inside example with a div
$("#example").empty();//remove all child nodes of example 
$("#example").remove();//remove example element and all child nodes
$("#example p.a, #example p.b").detach();
//like remove; but keep all data and event handler; use in case we need to put elements back
$("<div>replaced</div>").replaceAll("#example p[id]");//replace all p that have id attributes inside example element with "<div>replaced</div>"
$("#example p[id]").replaceWith("<div>replaced</div>");//like replaceAll
$("#example p").replaceWith(replacementFn);//but can use with callback function

function replacementFn() {
    if ($(this).text().indexOf("1") != -1) {//if textContent contains "1"
        return "<p>This is paragraph uno</p>";
    }
    else {
        return this.outerHTML;//no change
    }
}
```

* Manipulating attributes
```JavaScript
$("img").each(function(){
    console.log($(this).attr("alt"));
});//log all alt attributes of img elements

$("img").attr("title", "Set all img title attribute");
$("img").attr("class", "className");//set all img class attributes to "className"

//set multiple attributes at same time
$("img").attr({
    title: "Set all img title attribute",
    class: "className"
});

$("img").removeAttr("class");//remove all class attributes of all img elements 

```
* Working with CSS

Syntax|Meaning
---|---
`css(propName)`|get value of propName property
`css(propName, value)`|set value of propName property
`css(propName, function())`|set value of propName property to the result of function()
`css(JSON)`|set multiple property values at one
`hasClass(className)`|whether the element has className
`addClass(className | function())`|set class attribute to "className"
`removeClass("className" | function())`|remove class attribute value "className" 
`toggleClass(className | function())`|toggle the class attribute that has value "className"

```JavaScript
$("p").css("font-weight", "bold");
$("p").css("color", "red");
$("p").css("font-size", "+=1pt");
//or
$("p").css("font-weight", "bold").css("color", "red").css("font-size", "+=1pt");
//or
$("p").css({
    "font-weight": "bold",
    "color": "red",
    "font-size": "+=1pt"//increase by 1pt 
});
```

* Embedding custom data to page element
```JavaScript     
// <div id="example" data-key3="data attribute">

$("#example").data();//return {key3: "data attribute"}

$("#example").data("key1", 1234);
$("#example").data("key2", "Joe Marini");
//or
$("#example").data({"key1": 1234,"key2": "Joe Marini"});

$("#example").data("key1");//return 1234
$("#example").data();//return {"key3":"data attribute","key1":1234,"key2":"Joe Marini"}

$("#example").removeData("key2");
$("#example").data();//return {"key3":"data attribute","key1":1234}
```

## Events
* Binding and unbinding events with on() and off()
```JavaScript
//#evtTarget: a div
$("#evtTarget").on("mouseover mouseleave", highlight);//binding event handler with named function

$("#evtTarget").on("click", function(evt){
    $("#evtTarget").off("mouseover mouseleave", highlight);//unbinding event handler
    $("#evtTarget").html("<p>Hover effect shut off</p>");
    $("#evtTarget").removeClass("highlighted");
});//binding event handler with anonymous function

//#textEntry: a input type text
//#keyPress: a span
$("#textEntry").on("keypress", function(evt){
    $("#keyPress").text(String.fromCharCode(evt.charCode));        
});

function highlight(){
    $("#evtTarget").toggleClass("highlighted");
}
```
* Helper features 
    * There are a lot more at <https://api.jquery.com/category/events/>
```JavaScript
$("#evtTarget").hover(highlight, highlight);

$("#evtTarget").click(fnClick1);

$("#evtTarget").dblclick(fnClick2);

$(window).resize(fnResize);

$("#evtTarget").one("click", function() {
    $(this).css({ borderWidth: "4px",
        cursor: "pointer"
    });
});//one() run binding function only one time

function highlight(evt) {
    $("#evtTarget").toggleClass("highlighted");
}
function fnClick1() {
    $("#evtTarget").html("Click");
}
function fnClick2() {
    $("#evtTarget").html("Double Click");
}
function fnResize() {
    $("#evtTarget").html("Browser window resized");
}
```
* jQuery Event Object
    * `event.originalEvent`: native event object
```JavaScript
$("#Div1").on("click dblclick", { name: "Div 1" }, function(evt) {
    updateEventDetails(evt);
    evt.stopPropagation();
});
$("#Div2").on("click dblclick", { name: "Div 2" }, function(evt) {
    updateEventDetails(evt);
    evt.stopPropagation();
});
$("#Div3").on("click dblclick", { name: "Div 3" }, function(evt) {
    updateEventDetails(evt);
    evt.stopPropagation();
});
$("div.smallbox").on("mouseenter", function(evt) {
    updateEventDetails(evt);
    evt.stopPropagation();
});
$("div.smallbox").on("mouseleave", function(evt) {
    updateEventDetails(evt);
    evt.stopPropagation();
});
$("div.smallbox").on("mousemove", function(evt) {
    updateEventDetails(evt);
    evt.stopPropagation();
});

$("#input1").keypress(updateEventDetails);

function updateEventDetails(evt) {
    // clear any current text before we update the value fields
    $(".detailLine span[id]").text("");

    $("#evtType").text(evt.type);
    $("#evtWhich").text(evt.which);
    $("#evtTarget").text(evt.target.id);
    if (evt.relatedTarget)
        $("#evtRelated").text(evt.relatedTarget.tagName+"#"+evt.relatedTarget.id);
    $("#evtPageX").text(evt.pageX);
    $("#evtPageY").text(evt.pageY);
    $("#evtClientX").text(evt.clientX);
    $("#evtClientY").text(evt.clientY);
    $("#evtMetaKey").text(evt.metaKey);
    if (evt.data)
        $("#evtData").text(evt.data.name);
}
```

## Animation

```JavaScript
//show/hide
$("#show").click(function() {
    $("#theDiv").show("normal");
});
$("#hide").click(function() {
    $("#theDiv").hide(1000, "swing");
});
$("#toggle").click(function() {
    $("#theDiv").toggle("slow", completion);
});

//fade 
$("#fadein").click(function() {
    $("#theDiv").fadeIn("normal");
});
$("#fadeout").click(function() {
    $("#theDiv").fadeOut("normal");
});
$("#fadeto3").click(function() {
    $("#theDiv").fadeTo("slow", 0.3);
});
$("#fadeup").click(function() {
    $("#theDiv").fadeTo("slow", 1.0, onComplete);
});
$("#pulse").click(function() {
    $("#theDiv").fadeTo("fast", 0.3)
                .fadeTo("fast", 1.0)
                .fadeTo("fast", 0.3)
                .fadeTo("fast", 1.0);
});

//slide
$("#slideup").click(function() {
    $("#theDiv").slideUp(1000);
});
$("#slidedown").click(function() {
    $("#theDiv").slideDown(200);
});
$("#toggle").click(function() {
    $("#theDiv").slideToggle("slow");
});

//animate
$("#right").click(function() {
    $("#theDiv").animate({ width: "500px" }, 1000);
});
$("#text").click(function() {
    $("#theDiv").animate({ fontSize: "24pt" }, 1000);
});
$("#move").click(function() {
    $("#theDiv").animate({ left: "500" }, 1000, "swing");
});
$("#all").click(function() {
    $("#theDiv").animate({ left: "500", fontSize: "24pt", width: "500px" }, 1000, "swing");
});

//wait until animation finish
$("#theDiv").promise().done(function (){});
```

## AJAX
```JavaScript
$("document").ready(function() {
    getData();
    getData1();
    getXMLData();
    getJSONData();
});

function getData() {
    $.ajax({
        // the URL for the request
        url: "testdata.txt",

        // whether this is a POST or GET request
        type: "GET",
        
        // the type of data we expect back
        dataType : "text",
        
        // function to call for success
        success: successFn,

        // function to call on an error
        error: errorFn,
                        
        // code to run regardless of success or failure
        complete: function( xhr, status ) {
        console.log("The request is complete!");
        }
    });
}

function getData1() {
    $.ajax(
        // use the get() shorthand method to request a resource
        $.get("testdata.txt", successFn);

        // the load() shorthand method performs the common task of retrieving HTML content
        // and inserts the returned content into the specified element. 
        $("#content").load("testdata.html");
        
        //can also use jQuery selector on return result
        $("#content").load("testdata.html #p2");
    );
}

function successFn(result) {
    console.log("Setting result");
    $("#ajaxContent").append(result);
}
function errorFn(xhr, status, strErr) {
    console.log("There was an error!");
}

/* testxmldata.xml content
<data>
	<name>Joe Marini</name>
	<title>jQuery Essential Training</title>
</data>
*/

function getXMLData() {
    $.get("testxmldata.xml", function(result) {
        var title = result.getElementsByTagName("title")[0];
        var name = result.getElementsByTagName("name")[0];
        var val = title.firstChild.nodeValue + " by " + name.firstChild.nodeValue;
        $("#ajaxContent").append(val);
    });
}

function getJSONData() {
    var flickrAPI = "http://api.flickr.com/services/feeds/photos_public.gne?jsoncallback=?";
    $.getJSON( flickrAPI, {
        tags: "space needle",
        tagmode: "any",
        format: "json"
        },
        successFn);
}//http://api.flickr.com/services/feeds/photos_public.gne?jsoncallback=?tags=space%20needle&tagmode=any&format=json

function successFn(result) {
    $.each(result.items, function(i, item) {
        $("<img>").attr("src", item.media.m).appendTo("#ajaxContent");
        if (i === 4) {
            return false;
        }
    });
}
```
![jQuery AJAX global event handler](/img/ajax-global-event-handler.png)

```JavaScript
$("document").ready(function() {
    $(document).ajaxStart(function () {
        console.log("AJAX starting");
    });

    $(document).ajaxStop(function () {
        console.log("AJAX request ended");
    });

    $(document).ajaxSend(function (evt, jqXHR, options) {
        console.log("About to Send request for data...");
    });

    $(document).ajaxComplete(function (evt, jqXHR, options) {
        console.log("Everything's finished!");
    });

    $(document).ajaxError(function (evt, jqXHR, settings, err) {
        console.log("Hmmm. Seems like there was a problem: " + err);
    });

    $(document).ajaxSuccess(function (evt, jqXHR, options) {
        console.log("Looks like everything worked!");
    });

    getData();
});

function getData() {
    $.get("testdata2.txt", successFn);
}

function successFn(result) {
    console.log("Setting result");
    $("#ajaxContent").append(result);
}
```