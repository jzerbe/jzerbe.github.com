---
layout: post
title: deserialize nested JSON objects in Spring MVC with Jackson
tags:
- JSON
- Jackson
- Java
published: true
---
**NOTE**: I had to use [Jackson 2.2](http://repo1.maven.org/maven2/com/fasterxml/jackson/core/) to get this working.
[Jackson 1.9](http://repository.codehaus.org/org/codehaus/jackson/) did not cut it.


###API needs

The `VenueObj` has a single `LocationObj` field, called `location`.
The `LocationObj` field needs to be set when deserializing a full `VenueObj` JSON POST payload, and the `location` field
needs to be serialized when the `VenueObj` is serialized.


###VenueController.java POST mapping - Controller

    @RequestMapping(value = RequestMappingConstants.kApiVenues, method = RequestMethod.POST)
    protected @ResponseBody long putVenue(@RequestBody final VenueObj theVenueObj, final Principal thePrincipal, final HttpServletResponse response);

Note the annotation driven mapping. May need to wire up your
`[spring-dispatcher-servlet.xml](https://github.com/jzerbe/spring-security-gwt-template/blob/master/WEB-INF/spring-dispatcher-servlet.xml)`.


###VenueObj.java constructor - JSON de/serialization and ORM

    @JsonCreator
    public VenueObj(@JsonProperty(VenueObj.NAME) final String theName,
                    @JsonProperty(VenueObj.LOCATION) final LocationObj theLocation,
                    @JsonProperty(VenueObj.PHONE) final String thePhone,
                    @JsonProperty(VenueObj.WEB) final String theWeb) {
        
        this.name = theName;
        this.location = theLocation;
        this.phone = thePhone;
        this.web = theWeb;
        
    }

All other `public` constructors should be annotated with
`@JsonIgnore` to prevent the Jackson process from attempting to
deserialize with the incorrect one and spitting up.


###LocationObj.java getters/setters - JSON de/serialization and ORM

    @JsonProperty(LocationObj.LAT)
    public float getLat() {
        return this.lat;
    }
    
    @JsonProperty(LocationObj.LON)
    public float getLon() {
        return this.lon;
    }
    
    ...
    
    @JsonProperty(LocationObj.LAT)
    public void setLat(final float theLat) {
        this.lat = theLat;
    }
    
    @JsonProperty(LocationObj.LON)
    public void setLon(final float theLon) {
        this.lon = theLon;
    }

The `LocationObj` class is a constructor-less cacophony of getters
and setters, with a few `private` data manipulation methods.


###Conclusion

And now the service should be able to make sense of incoming JSON like:

    {
        "name":"Eldora Mountain Resort",
        "location":{
            ...
            "lat":39.937678,
            "lon":-105.580888,
            ...
        },
        "phone":"(303) 440-8700",
        "web":"http://www.eldora.com/"
    }


###Further Reading

- [Jackson Annotations: @JsonCreator demystified](http://www.cowtowncoder.com/blog/archives/2011/07/entry_457.html)
