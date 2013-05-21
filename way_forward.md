# A "WAY" FORWARD

Several months ago our lab was tasked with assisting Indonesia's electric utility company in digitally capturing a subset of their massive power grid.  The data would then be used as an input to our [NetworkPlanner](http://networkplanner.modilabs.org) tool to create a broad electrification plan.  A side benefit would be that PLN gets a system to maintain and update their grid data.  This sounded like a fantastic opportunity (at least to me), but it came with a couple of catches:  

1.  The work was to start immediately (2 of our bravest were already in Indonesia planning the work)
2.  We had no system for capturing such data

So what do rational developers do in such circumstances?  

## STALL...

Or maybe create a distraction and hide out until a solution reveals itself.  Hmm...but we couldn't leave our colleagues out to dry.  It seemed we were stuck.  

So, the picture painted above is a little meladramatic.  We actually have a proven set of tools for data collection centered around [FormHub](http://formhub.org "FormHub").  And our initial approach was to utilize a combination of FormHub for salient point (Transformers, Sub-Stations) capture and a GPS tracking application called OSMTracker to capture the line data (Medium-Voltage line to be specific).  In theory, this was all we needed to meet our goals...as a former-professor of mine and Googler once said to me, "it's just data" (I think he was trying to advise me).  In our case, it's just points and lines, right?  How hard could it be to stitch them together into a seamless network?  

## ACTUALLY...

That's kind of difficult.  In our initial training with PLN, we did our best to impose a structure that would allow us to piece together the network data accurately at a later point.  This involved being disciplined about starting and stopping tracking at appropriate points and capturing branch points with the number of segments as a sort of validation check [needs elaboration].  In the end, what we captured amounted to a plate of [cartographic spaghetti](http://support.esri.com/en/knowledgebase/GISDictionary/term/spaghetti%20data "Spaghetti Data").  We could work with it, but organizing it and cleaning it up was going to be time consuming.  

## A SOLUTION...

In [The Mythical Man Month](http://en.wikipedia.org/wiki/The_Mythical_Man-Month "Mythical Man Month"), Brooks demonstrates that in software development, the cost of a bug increases throughout its lifetime.  A similar rule seems to hold for data collection processes as well.  Basically, its cheaper to capture the data directly in a form that makes it most useful.  [elaborate this point]  What we wanted was a more integrated system and structure for capturing geometry.    Several of us in the lab had some experience working with OpenStreetMap.  On the surface, it might seem hard to argue against using OSM.  It's an Open Source, distributed spatial data collection system supported by a large, smart, practical and friendly community.  But the argument is more fundamental than that.  

I can't do it justice in this post, but at it's core, OSM is a topological data model abstracted as Nodes (Points), Ways (which reference Nodes) and Relations (which reference Nodes, Ways or other Relations).  This model affords much reduced effort in maintaining the integrity of geometry vs our prior approach or via managing independent shape files of points and lines.  Additionally, OSM is a "write-once" model with versioning (so the entire history of data is retained)...a nice safety-net.  And with tools like JOSM to edit this geometry and interact with the OSM repository our job seemed almost done.  

## OR SO WE THOUGHT...

After extolling the virtues of OpenStreetMap and it's topological elegance I was brought back to Earth by my colleagues and the powers that be by two remaining issues:

1.  It was simpler to train and use FormHub for point and attribute data capture
2.  Data that goes into OSM is bound by the [Open Database License](http://opendatacommons.org/licenses/odbl/summary/ "ODbL").  That basically meant we would need to convince PLN to publicize their data from the start.  

To resolve #1 we decided to continue to use FormHub to collect the salient points of the grid.  We'd need to come up with a scheme for synchronizing between FormHub and our OpenStreetMap solution, but we'll get to that.  

The one obvious solution to #2 was to deploy our own instance of the OpenStreetMap server and database (called the [Rails Port](http://wiki.openstreetmap.org/wiki/The_Rails_Port).  This was NOT an easy decision.  It would require us to maintain our instance WITHOUT the benefits of a wholly integrated repository.  In the end, we decided that the flexibility of our own instance was worth the effort and we began setting up.  

## THE UGLY DUCKLING...

As we neared our 2nd round of training and data capture (in which 4 of us would be participating in on the island of West Timor, Indonesia), it was becoming apparent that our "system" was a bit of a Frankenstein.  It was difficult for us engineers back in NY to come to an understanding of how this system would work.  The OpenStreetMap piece was fairly new to us and not at all trivial.  The synchronization scheme was not well defined.  How could we introduce this Ugly Duckling of a system as our solution?  

## SIMPLE...

Apply [Fire and Motion](http://www.joelonsoftware.com/articles/fog0000000339.html "Fire and Motion").  We had done our research, design and just had a few more things to do to get the system "working".  To solve synchronization, we wrote a script to diff the FormHub data with a "synchronization db" and push the new data into our OpenStreetMap instance.  On planes and in the evenings during the training, we tailored our FormHub forms to be more consistent with the OpenStreetMap tagging paradigm.  We customized JOSM to display icons denoting specific power features like Sub-Stations and End-Poles.  And then we used the system to capture the data we needed.  We even made time to step on a few sea-urchins, get treated at a local health-clinic, surf some incredible waves (including an off-shore reef), scuba-dive and just experience a beautiful country and culture.  

## A SWAN?

Was it elegant?  Hardly.  Was it effective?  We like to think so.  We used it to capture and integrate > 1000km of grid data into a cohesive data-set that can now be updated by any of its users.  Will this Ugly Duckling turn into a swan?  Time will tell ;)  
