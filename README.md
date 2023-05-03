Download Link: https://assignmentchef.com/product/solved-cs2030s-lab-2-cruise-loaders
<br>
The Kent Ridge Cruise Centre has just opened and you are required to design a program to decide how many loaders to buy based on a single-day cruise schedule.

CruisesEach cruise has four attributes:

a unique string identifier, e.g. S1234a time of arrival in HHMM format, e.g. 2359 denoting the cruise arriving at 11:59PM on that daythe time needed to serve the cruise (in minutes), andthe number of loaders needed to serve the cruise.Every cruise must be served by loaders immediately upon arrival.

There are two types of cruises:

SmallCruise:has an identifier that starts with an uppercase character S;takes a fixed 30 minutes for a loader to fully load;requires only one loader for it to be fully served;BigCruise:has an identifier that starts with an uppercase character B;takes one minute to serve every 50 passengers;requires one loader per every 40 meters in length of the cruise (or part thereof) to fully loadLoadersLoaders have to be purchased to serve cruises. Each loader comprises two attributes:

a unique integer identifierthe cruise that it is currently servingA loader will serve a cruise as soon as it arrives, and continues to do so until the service time has elapsed (i.e. it cannot serve a cruise while in the midst of serving another one).

For example, if an incoming cruise arrives at 12PM, requires two loaders, and 60 min for it to be fully served, then, at 12PM, there must be two vacant loaders. These two loaders will serve the cruise from 12PM – 1PM. They can only serve another cruise from 1PM onwards.

TaskYour task is to determine the loader allocation schedule using the following steps:

For each cruise, check through the inventory of existing loaders, starting from the loader first purchased, and so on;The first (or first few) loaders available will be used to serve the cruise;If there are not enough loaders, purchase new one(s) to serve the cruise.Take note of the following assumptions:

Input cruises are presented chronologically by arrival time.There can be up to 30 cruises in one day.The number of loaders servicing a cruise will not exceed 9.There are no duplicate cruises.All cruises will arrive and be completely served within a single day..Although this problem can be implemented procedurally, you are to model your solution using an object-oriented approach instead.

This task is divided into five levels. You need to complete ALL levels.

Level 1Represent a CruiseDesign an immutable Cruise class to represent a cruise having a unique identifier string, the time of arrival as an integer, the number of loaders required to load the cruise as an integer, and the service time in minutes as an integer.

class Cruise {private final String identifier;private final int arrivalTime;private final int numOfLoader;private final int serviceTime;

…}

Note that the time of arrival is in HHMM format. Specifically, 0 or 0000 refers to 00:00 (12AM), 30 or 0030 refers to 00:30 (12:30AM), and 130 or 0130 refers to 01:30 (1:30AM).

Implement a getServiceCompletionTime method, which returns the time the service completes (in number of minutes) since midnight, and a getArrivalTime method, which returns the arrival time (in number of minutes) since midnight.

For example, if the cruise arrives at 12PM (noon time), the arrival time is (12 * 60) = 720; the service completion time is 12:30PM, which is 750 minutes since midnight, i.e. (12 * 60) + 30 = 750.

In addition, implement a getNumOfLoadersRequired method, which returns the number of loaders required to load the cruise.

A string representation of a cruise is in the form:

<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="d7b4a5a2bea4b29e93979f9f9a9a">[email protected]</a>

The %0Xd format specifier might be of use to you, where the integer will be represented by an X-digit zero-padded number. For instance,String.format(“%04d”, 20);

would return the string 0020.jshell&gt; /open Cruise.javajshell&gt; new Cruise(“A1234”, 0, 2, 30)$.. ==&gt; <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="edacdcdfded9addddddddd">[email protected]</a>jshell&gt; new Cruise(“A2345”, 30, 2, 30)$.. ==&gt; <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="beff8c8d8a8bfe8e8e8d8e">[email protected]</a>jshell&gt; new Cruise(“A3456”, 130, 2, 30)$.. ==&gt; <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="5514666160631565646665">[email protected]</a>jshell&gt; new Cruise(“A3456”, 130, 2, 30).getArrivalTime()$.. ==&gt; 90jshell&gt; new Cruise(“A3456”, 130, 2, 30).getNumOfLoadersRequired()$.. ==&gt; 2jshell&gt; new Cruise(“A3456”, 130, 5, 30).getNumOfLoadersRequired()$.. ==&gt; 5jshell&gt; new Cruise(“A1234”, 0, 2, 30).getServiceCompletionTime()$.. ==&gt; 30jshell&gt; new Cruise(“A1234”, 0, 2, 45).getServiceCompletionTime()$.. ==&gt; 45jshell&gt; new Cruise(“CS2030”, 1200, 2, 100).getServiceCompletionTime()$.. ==&gt; 820jshell&gt; new Cruise(“D1010”, 2329, 2, 30).getServiceCompletionTime()$.. ==&gt; 1439jshell&gt; /exit

Check the format correctness of the output by running the following on the command line:

$ javac *.java$ jshell -q &lt; test1.jsh

Check your styling by issuing the following$ checkstyle *.java

Level 2Use Loaders to serve CruisesDesign an immutable Loader class to serve a cruise.

class Loader {private final int identifier;private final Cruise cruise;

…}

Include the following in the Loader class:

a constructor that takes in an integer denoting its unique identifier, as well as the first cruise that it serves.a canServe(Cruise) method that returns true if the loader is available to serve the given cruise, or false otherwise.a serve(Cruise) method to serve a given cruise. If the loader is available, the method returns the loader serving this cruise; otherwise the existing loader is returnedthe methods getIdentifier() and getNextAvailableTime() to return the loader’s identifier, and the next available time for service.The string representation of each Loader is in the form:

Loader ID serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="f29180879b8197bbb6b29180879b81979380809b84939e869b9f97">[email protected]</a>

jshell&gt; /open Cruise.javajshell&gt; /open Loader.javajshell&gt; new Loader(1, new Cruise(“A1234”, 0, 1, 30))$.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="e1a0d0d3d2d5a1d1d1d1d1">[email protected]</a>jshell&gt; new Loader(1, new Cruise(“A1234”, 0, 1, 30)).getIdentifier()$.. ==&gt; 1jshell&gt; new Loader(1, new Cruise(“A1234”, 0, 1, 30)).getNextAvailableTime()$.. ==&gt; 30jshell&gt; new Loader(1, new Cruise(“A1234”, 0, 1, 30)).canServe(new Cruise(“A2345”, 30, 1, 30))$.. ==&gt; truejshell&gt; new Loader(1, new Cruise(“A1234”, 0, 1, 30)).serve(new Cruise(“A2345”, 30, 1, 30))$.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="d392e1e0e7e693e3e3e0e3">[email protected]</a>jshell&gt; new Loader(1, new Cruise(“A1234”, 0, 1, 30)).serve(new Cruise(“A2345”, 30, 1, 30)).getNextAvailableTime()$.. ==&gt; 60jshell&gt; new Loader(1, new Cruise(“A1234”, 0, 1, 30)).canServe(new Cruise(“A2345”, 10, 1, 30))$.. ==&gt; falsejshell&gt; new Loader(1, new Cruise(“A1234”, 0, 1, 30)).serve(new Cruise(“A2345”, 10, 1, 30))$.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="e3a2d2d1d0d7a3d3d3d3d3">[email protected]</a>jshell&gt; new Loader(1, new Cruise(“A1234”, 0, 1, 30)).serve(new Cruise(“A2345”, 10, 1, 30)).getNextAvailableTime()$.. ==&gt; 30jshell&gt; new Loader(1, new Cruise(“A1234”, 0, 1, 30)).serve(new Cruise(“A2345”, 10, 1, 30)).getIdentifier()$.. ==&gt; 1jshell&gt; /exit

Check the format correctness of the output by running the following on the command line:

$ javac *.java$ jshell -q &lt; test2.jsh

Check your styling by issuing the following$ checkstyle *.java

Level 3Represent Small CruisesDesign the SmallCruise class. The arguments of the constructor are its identifier, and time of arrival. Note that you should not need to change your Loader class if you have implemented it properly.

jshell&gt; /open Loader.javajshell&gt; /open Cruise.javajshell&gt; /open SmallCruise.javajshell&gt; new SmallCruise(“S0001”, 0).getArrivalTime()$.. ==&gt; 0jshell&gt; new SmallCruise(“S0001”, 0).getServiceCompletionTime()$.. ==&gt; 30jshell&gt; new SmallCruise(“S0001”, 0).getNumOfLoadersRequired()$.. ==&gt; 1jshell&gt; (Cruise) new SmallCruise(“S0123”, 1220)$.. ==&gt; <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="5506656467661564676765">[email protected]</a>jshell&gt; new Loader(1, new SmallCruise(“S1245”, 2330))$.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="c695f7f4f2f386f4f5f5f6">[email protected]</a>jshell&gt; new Loader(1, new SmallCruise(“S1245”, 2330)).canServe(new SmallCruise(“S2345”, 2359))$.. ==&gt; falsejshell&gt; new Loader(1, new SmallCruise(“S1245”, 2330)).serve(new SmallCruise(“S2345”, 2359))$.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="9bc8aaa9afaedba9a8a8ab">[email protected]</a>jshell&gt; new Loader(1, new SmallCruise(“S2030”, 0))$.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="92c1a0a2a1a2d2a2a2a2a2">[email protected]</a>jshell&gt; /exit

Check the format correctness of the output by running the following on the command line:

$ javac *.java$ jshell -q &lt; test3.jsh

Check your styling by issuing the following$ checkstyle *.java

Level 4Represent Big CruisesDesign the BigCruise class. The arguments of the constructor are its identifier, time of arrival, the length of the cruise, and number of passengers, in that order.

jshell&gt; /open Loader.javajshell&gt; /open Cruise.javajshell&gt; /open SmallCruise.javajshell&gt; /open BigCruise.javajshell&gt; Cruise b = new BigCruise(“B0001”, 0, 70, 3000)jshell&gt; b.getArrivalTime()$.. ==&gt; 0jshell&gt; b.getServiceCompletionTime()$.. ==&gt; 60jshell&gt; b.getNumOfLoadersRequired()$.. ==&gt; 2jshell&gt; new Loader(1, b).serve(b)$.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="c082f0f0f0f180f0f0f0f0">[email protected]</a>jshell&gt; new Loader(1, b).serve(b).getNextAvailableTime()$.. ==&gt; 60jshell&gt; new Loader(2, b)$.. ==&gt; Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="2e6c1e1e1e1f6e1e1e1e1e">[email protected]</a>jshell&gt; new Loader(3, b)$.. ==&gt; Loader 3 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="4d0f7d7d7d7c0d7d7d7d7d">[email protected]</a>jshell&gt; new Loader(4, new BigCruise(“B2345”, 0, 30, 1450)).serve(new SmallCruise(“S0000”, 29))$.. ==&gt; Loader 4 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="2f7c1f1f1f1f6f1f1f1d16">[email protected]</a>jshell&gt; new Loader(5, new BigCruise(“B3456”, 0, 75, 1510)).serve(new SmallCruise(“S0001”, 30))$.. ==&gt; Loader 5 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="cd8ffef9f8fb8dfdfdfdfd">[email protected]</a>jshell&gt; /exit

Check the format correctness of the output by running the following on the command line:

$ javac *.java$ jshell -q &lt; test4.jsh

Check your styling by issuing the following$ checkstyle *.java

Level 5Output the loader allocation scheduleWrite a method serveCruises(Cruise[]) that takes in an array of cruises, and outputs the allocation schedule of the loaders required to service all the cruises. Save the method in the file level5.jsh.

You may assume that there are at most 30 cruises in one day, and the number of loaders servicing a cruise will not exceed 9.

jshell&gt; /open Cruise.javajshell&gt; /open SmallCruise.javajshell&gt; /open BigCruise.javajshell&gt; /open Loader.javajshell&gt; /open level5.jshjshell&gt; Cruise[] cruises = {…&gt;     new SmallCruise(“S1111”, 1300)}jshell&gt; serveCruises(cruises);Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="a3f092929292e392909393">[email protected]</a>jshell&gt; Cruise[] cruises = {…&gt;     new BigCruise(“B1111”, 1300, 80, 3000),…&gt;     new SmallCruise(“S1111”, 1359),…&gt;     new SmallCruise(“S1112”, 1400),…&gt;     new SmallCruise(“S1113”, 1429)}jshell&gt; serveCruises(cruises);Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="084a3939393948393b3838">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="c486f5f5f5f584f5f7f4f4">[email protected]</a>Loader 3 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="a4f795959595e49597919d">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="bcef8d8d8d8efc8d888c8c">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="4c1f7d7d7d7f0c7d787e75">[email protected]</a>jshell&gt; Cruise[] cruises = {…&gt;     new SmallCruise(“S1111”, 900),…&gt;     new BigCruise(“B1112”, 901, 100, 1),…&gt;     new BigCruise(“B1113”, 902, 20, 4500),…&gt;     new SmallCruise(“S2030”, 1031),…&gt;     new BigCruise(“B0001”, 1100, 30, 1500),…&gt;     new SmallCruise(“S0001”, 1130)}jshell&gt; serveCruises(cruises);Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="b3e082828282f3838a8383">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="3072010101027000090001">[email protected]</a>Loader 3 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="ca88fbfbfbf88afaf3fafb">[email protected]</a>Loader 4 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="d99be8e8e8eb99e9e0e9e8">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="67255656565427575e5755">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="1f4c2d2f2c2f5f2e2f2c2e">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="e7a5d7d7d7d6a7d6d6d7d7">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="5003606060611061616360">[email protected]</a>jshell&gt; /exit

Check the format correctness of the output by running the following on the command line:

$ javac *.java$ jshell -q &lt; test5.jsh

Check your styling by issuing the following$ checkstyle *.java

Level 6Recycled LoadersThe objective of this level is to determine whether your current implementation can be easily extended with minimal modification to the client

The Cruise Centre has just introduced a new eco-friendly policy in an effort to go green. Their policy states that every third loader that is purchased must be made of recycled materials (referred to as recycled loaders). These recycled loaders will go through a 60-minute long maintenance after every service. It is unable to serve any cruise during this period.

For example, if a recycled loader serves a SmallCruise that arrives at 12:30PM, then the next time the loader can serve another Cruise is 2PM (30min + 60min after 12:30PM).

By modifying level5.jsh, incorporate the above with minimal modifications to the file level6.jsh.

jshell&gt; /open Cruise.javajshell&gt; /open SmallCruise.javajshell&gt; /open BigCruise.javajshell&gt; /open Loader.javajshell&gt; /open RecycledLoader.javajshell&gt; /open level6.jshjshell&gt; Cruise[] cruises = {…&gt;     new BigCruise(“B1111”, 0, 60, 1500),…&gt;     new SmallCruise(“S1112”, 0),…&gt;     new BigCruise(“B1113”, 30, 100, 1500),…&gt;     new BigCruise(“B1114”, 100, 100, 1500),…&gt;     new BigCruise(“B1115”, 130, 100, 1500),…&gt;     new BigCruise(“B1116”, 200, 100, 1500)…&gt; }jshell&gt; serveCruises(cruises);Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="f6b4c7c7c7c7b6c6c6c6c6">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="d496e5e5e5e594e4e4e4e4">[email protected]</a>Recycled Loader 3 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="0e5d3f3f3f3c4e3e3e3e3e">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="4b097a7a7a780b7b7b787b">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="c684f7f7f7f586f6f6f5f6">[email protected]</a>Loader 4 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="efaddedededcafdfdfdcdf">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="1e5c2f2f2f2a5e2e2f2e2e">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="a9eb9898989de999989999">[email protected]</a>Loader 4 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="4406757575700474757474">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="e4a6d5d5d5d1a4d4d5d7d4">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="4507747474700575747675">[email protected]</a>Recycled Loader 3 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="d795e6e6e6e297e7e6e4e7">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="a4e695959592e494969494">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="7331424242453343414343">[email protected]</a>Loader 4 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="2466151515126414161414">[email protected]</a>jshell&gt; /exit

Check your styling by issuing the following$ checkstyle *.java