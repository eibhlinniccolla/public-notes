- To transfer an object via HTTP or to otherwise convert it to a string, it is necessary to _serialize_ it.
    - Objects can be serialized using the JSON.stringify function
- To _deserialize_ an object (convert it from a string to an object), you can use the JSON.parse function from the same json2 library.
- Examples:  
    var christmasList = {mike:"Book", jason:"sweater", chelsea:"iPad" }  
    JSON.stringify (christmasList);  
    ​// Prints this string:​  
    ​// "{"mike":"Book","jason":"sweater","chels":"iPad"}"    
    ​  
    ​// To print a stringified object with formatting, add "null" and "4" as parameters:​  
    JSON.stringify (christmasList, null, 4);  
    // "{  
    "mike": "Book",  
    "jason": "sweater",  
    "chels": "iPad"​  
    }"  
    ​  
    ​// JSON.parse Examples \\​  
     // The following is a JSON string, so we cannot access the properties with dot notation (like christmasListStr.mike)​  
    ​var christmasListStr = '{"mike":"Book","jason":"sweater","chels":"iPad"}';  
    ​  
    ​// Let’s convert it to an object​  
    ​var christmasListObj = JSON.parse (christmasListStr);  
    ​  
    ​// Now that it is an object, we use dot notation​  
    console.log(christmasListObj.mike); // Book