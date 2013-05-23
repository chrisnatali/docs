# A "WAY" FORWARD

As described in a previous post, our lab recently undertook the task of assisting Indonesia's electric utility company (PLN) in digitally capturing a subset of their massive power grid.  The data would then be used as an input to our [NetworkPlanner](http://networkplanner.modilabs.org) tool to create a broad electrification plan.  A potential by-product of this effort would be a system to maintain and update their grid data.  This post is about the evolution of that system from a software system engineering (?) perspective.    

### STRIKING WHILE THE IRON IS HOT

4 months ago, our Software Engineering team in NY was contacted by our Energy Planning team who was in Indonesia.  The details weren't too clear at first, but we understood that we had agreed to take on a project to map some Medium Voltage line for PLN.  For a brief moment, our envisioned timelines for this work were a little off.  The Software Engineering team was thinking in terms of weeks, but we really needed to be thinking in terms of days, even hours, because the training and data acquisition was to start in...2 days!  Were we really willing to sacrifice architectural elegance in order to take advantage of this opportunity?  [Architecture Astronauts](http://www.joelonsoftware.com/items/2008/05/01.html) unite!
  
### REALITY

OK, the situation wasn't THAT dire.  We actually have a proven set of tools for data collection centered around [FormHub](http://formhub.org "FormHub").  And our initial approach was to utilize a combination of FormHub for salient point (transformers, sub-stations) capture and a GPS tracking application called OSMTracker to capture the medium-voltage line data.  In theory, this was all we needed to meet our goals.  After all, it's just points and lines, right?  How hard could it be to stitch them together into a seamless network later?  

### ACTUALLY...

That's kind of cumbersome.  In our initial training with PLN, we made an attempt to impose a structure that would allow us to piece together the network data accurately at a later point.  This involved being disciplined about starting and stopping tracking at appropriate points, capturing branch points with the associated number of radiating segments and even naming the gps trace files according to the mv-line identity.  Again, the rational seemed sound, but what we captured was quickly amounting to a plate of [cartographic spaghetti](http://support.esri.com/en/knowledgebase/GISDictionary/term/spaghetti%20data "Spaghetti Data").  We could work with it, but organizing it and cleaning it up was quite laborious.  

### FILLING THE VOID

In [The Mythical Man Month](http://en.wikipedia.org/wiki/The_Mythical_Man-Month "Mythical Man Month"), Brooks demonstrates that in software development, the cost of a bug increases throughout its lifetime (so one should squash them early).  A similar rule seems to hold for data collection processes.  Maybe this is overgeneralizing and stating the obvious, but we found that the longer data sits in an unprocessed state and the further it gets from its source, the more costly it becomes to munge into something useful.  Just days after capturing data out in the field, it was difficult to piece together a network solely from gps traces and some salient points and attributes.  A more integrated system and structure for capturing geometry would help.  Several of us in the lab had positive experiences working with [OpenStreetMap](http://www.openstreetmap.org).  On the surface, it might seem hard to argue against using OSM for this work.  It's an Open Source, distributed spatial data collection system supported by a large, smart, practical and friendly community.  

I can't do it justice in this post, but OSM relies on a topological data model abstracted as Nodes (Points), Ways (which reference Nodes) and Relations (which reference Nodes, Ways or other Relations).  Nodes are the fundamental unit of composition, which affords much reduced effort in maintaining the integrity of geometry vs our prior approach or via managing independent shape files of points and lines.  Additionally, OSM data is versioned (so the history of data is retained)...a nice safety-net.  And with practical, efficient user interfaces like JOSM and the new [iD editor](http://ideditor.com) to manipulate this geometry and interact with the OSM repository, our job seemed almost done.  

### OR SO WE THOUGHT...

After extolling the virtues of OpenStreetMap and it's topological elegance we were brought back to Earth by two remaining issues:

1.  It was simpler to train and use FormHub for point and attribute data capture in the field.
2.  Data that goes into OSM is bound by the [Open Database License](http://opendatacommons.org/licenses/odbl/summary/ "ODbL").  That basically meant we would need to convince PLN to publicize their data from the start.  And even if we did, we still could not load much of the existing grid data we acquired from other sources due to license incompatibility.  

To resolve #1 we decided to continue to use FormHub to collect the salient points of the grid.  This necessitated a synchronization scheme between FormHub and our OpenStreetMap solution, but we'll get to that.  

The one obvious solution to #2 was to deploy our own instance of the OpenStreetMap server and database (called the [Rails Port](http://wiki.openstreetmap.org/wiki/The_Rails_Port).  This was NOT an easy decision.  It would require us to maintain our instance with an ill-defined path for merging source and data back to the master at some point.  In the end, we decided that the flexibility gained by having our own instance was worthwhile and we began setting it up.  

### THE UGLY DUCKLING

As our 2nd round of on-site training and data capture was quickly approaching, it was becoming apparent that our "system" was a bit of a Frankenstein.  The OpenStreetMap piece was fairly new to us and not at all trivial.  To solve synchronization, we manually ran a script to diff the FormHub data with a "synchronization db" and push the new data into our OpenStreetMap instance.  Not pretty.  How would we execute with this Ugly Duckling of a system?  

### EXECUTION

In hindsight, our strategy resembled [Fire and Motion](http://www.joelonsoftware.com/articles/fog0000000339.html "Fire and Motion").  We basically focused on incremental progress in a "damn the torpedoes" style (too much?).  We tailored our FormHub forms to be more consistent with the OpenStreetMap tagging paradigm.  We customized JOSM to display icons denoting specific power features like Sub-Stations and End-Poles...a simple change with dramatic effect.  

And then we used the system to capture the data we needed as we trained PLN on its use.  We even made time to step on a few sea-urchins, get treated at a local health-clinic, surf some incredible waves (including an off-shore reef), scuba-dive some fantastic water and simply experience a beautiful country and culture.  

## A SWAN?

Was it elegant?  Hardly.  Effective?  We like to think so.  We used it to capture and integrate over 2300 km of grid data into a cohesive dataset that can now be updated by any of its users.  Will this Ugly Duckling turn into a swan?  Time will tell.  
