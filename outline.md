# Title: Analyzing Stormwater Best Management Plan Data #



* Subtitle 1: Matryoshka doll (get photo)
* Subtitle 2: Shrimp and Grits
* Subtitle 3: Russia is a riddle wrapped in a mystery inside an enigma.
* Subtitle 4: Data encased in lists, wrapped in vectors, swaddled in JSON
* Date: 17 Oct 2023
* Author: Erik Beck


# Opening Photo and Caption #
* (https://images.freeimages.com/images/large-previews/6a4/russian-nesting-doll-1187383.jpg)
* Captions

# Overview #

* Speedy Talk: Quick Overview of
   + Stormwater::CWA 319::BMPs::GRTS
   + Why the GRTS System? -> Strings attached to Govt. money
   + Oracle Business Intelligence: Clunky and not very capable
   + Basic questions to answer:
        - What is the mean level of funding for Nonpoint Source projects in New England (and the confidence interval)?
		- How is that funding scattered across appropriation years? What does that look like graphically?
		- Can't answer these basic questions using OBI

# Stormwater, CWA 319, Nonpoint Source Pollution, BMPs, and GRTS #
* EPA's GRTS system a required part of getting EPA funding under CWA 319 (NPS)
* States have to enter the data on projects using EPA funding 
* Entered data is broad info on work done under EPA grants
* Data goes into Grants Results and Tracking System (GRTS)
* OW/OWOW builds and maintains the system
    + Data entered by states, tribes, regions
	+ System can export some data 'natively' to PDF and CSV, but pretty limited

# Getting the Data to R #
* OW has developed one Application Programming Interface (API) to GRTS
     + More in development
	 + Uses HUC12 to find data
	 + One to many relationships of data
	 + One HUC may have zero to many BMP projects associated with it across years, grants, etc
* Using the API returns a single JSON structure for a HUC12
     + JSON is deeply nested; challenge is picking apart the material and massaging it for analysis

# Example: One disentanglement function of several #
	;;HUCmetaData <- function (DataRequestRaw) {

    ;; Grab the metadata

    HUCdataBlobRaw <- DataRequestRaw$content
    HUCdataBlob <- rawToChar(HUCdataBlobRaw)
    
    HUCdataBlob %>% spread_all -> HUCmetaDataBall

    document.id <- HUCmetaDataBall$document.id # document id (integer). Sequential?
    hasMore <- HUCmetaDataBall$hasMore # Are more top-level records for this HUC?
    limit <- HUCmetaDataBall$limit # Limit of projects listed per HUC?
    offset <- HUCmetaDataBall$offset # Not sure what offset is
    count <- HUCmetaDataBall$count  # How many projects are in this HUC

    ;; The Payload is in 'HUCmetaDataBall$..JSON'. Will leave extraction of this data to a different function.
    
    HUCMetaData <- data.frame (document.id,hasMore,limit,offset,count) 
    return (HUCMetaData)
	;; }


# After the GRTS data is tamed, we can start answering the basic questions #

* What's the mean and 'data spread' of GRTS funding in New England?
     + Chart here

* What does the data look like when grouped by appropriation years?
     + Chart here
	 

# Conclusion #

* Using R to analyze GRTS data is very useful
* APIs need some work to provide more parts of the dataset
* Other APIs in development need to mature and move to the production system
* Working with the deeply nested JSON data is worth it; trying to
  coerce this info from OBI would be very difficult or impossible.
  
# Questions? #



