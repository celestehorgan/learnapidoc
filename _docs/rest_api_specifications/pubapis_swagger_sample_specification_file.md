---
title: "Sample Swagger specification file"
permalink: /pubapis_swagger_sample_specification_file.html
course: "Documenting REST APIs"
sidebar: docapis
weight: 8.26
section: restapispecifications
path1: /restapispecifications.html
---

In the [Swagger tutorial](pubapis_swagger.html), I referenced a Swagger contract that you simply plugged into a Swagger UI project. In this section, we'll dive more deeply into the Swagger specification file (but not too deep, because one could write an entire book on the subject).

The following is the Swagger specification file that I created for the [sample Mashape Weather API](https://market.mashape.com/fyhao/weather-13). Note that this API builds off of a [Yahoo weather service API](https://developer.yahoo.com/weather/documentation.html), so the data returned in the `weather` and `weatherdata` endpoints is highly similar to the data returned by the Yahoo weather service API.

After the specification file (which is provided in two versions of the spec -- 2.0 and 3.0), I'll go through and explain various parts of the spec and how to read the spec documentation.

{% if site.target == "web" %}
* TOC
{:toc}
{% endif %}

## Swagger 2.0 specification file

Version 2.0 is the current version supported by most Swagger UI tools.

{% include tip.html content="To understand the structure, it's helpful to be familiar with YAML syntax. See [More about YAML](pubapis_yaml.html) for an intro to YAML." %}

```yaml
swagger: "2.0"
info:
  description: "This is a sample API that uses a Mashape Weather API as an example to demonstrate features in the Swagger-2.0 specification. The Weather API displays forecast data by latitude and longitude. It's a simple weather API, but the data comes from Yahoo Weather Service. The weatherdata endpoint delivers the most robust and comprehensivepackage of weather information."
  version: 2.0.0
  title: Weather API from Mashape
  termsOfService: http://swagger.io/terms/
  contact:
    email: tom@idratherbewriting.com
  license:
    name: Apache 2.0
    url: http://www.apache.org/licenses/LICENSE-2.0.html
host: "simple-weather.p.mashape.com"
schemes:
- https
paths:
  /aqi:
    get:
      tags:
      - Air Quality
      summary: Gets the air quality index
      description: "Air quality is a measure of air pollution. The higher the number, the worse the pollution."
      operationId: getAqi
      produces:
      - text/plain
      parameters:
        - name: lat
          in: query
          description: latitude
          type: string
          required: true
        - name: lng
          in: query
          description: longitude
          type: string
          required: true
      responses:
        '200':
          description: AQI response
          schema:
            $ref: '#/definitions/aqiResponse'
          examples:
             application/text: 52
      security:
        - api_key: []


  "/weather":
    get:
      tags:
      - Weather Forecast
      summary: Gets the weather forecast in abbreviated form
      description: 'retrieves the weather forecast, but without too much detail'
      operationId: getWeather
      produces:
      - text/plain
      parameters:
        - name: lat
          in: query
          description: latitude coordinates
          required: true
          type: string
        - name: lng
          in: query
          description: longitude coordinates
          required: true
          type: string
      responses:
        '200':
          description: weather response
          schema:
            $ref: '#/definitions/weatherResponse'
          examples:
             application/text: "26 c, Mostly Clear at Singapore, Singapore"

      security:
        - api_key: []

  "/weatherdata":
    get:
      tags:
      - Weather Data
      summary: Get weather forecast with lots of details
      description: "Includes full details of the weather forecast, in JSON format"
      operationId: getWeatherData
      produces:
        - "application/json"
      parameters:
        - name: lat
          in: query
          description: latitude
          type: string
          required: true
        - name: lng
          in: query
          description: longitude
          type: string
          required: true
      responses:
        200:
          description: "Successful operation"
          schema:
           "$ref": "#/definitions/weatherdata response"
      security:
        - api_key: []

securityDefinitions:
  api_key:
    type: apiKey
    name: X-Mashape-Key
    in: header

externalDocs:
  description: Find out more about this Weather API
  url: https://market.mashape.com/fyhao/weather-13

definitions:
  aqiResponse:
    type: string

  weatherResponse:
    type: string

  weatherdata response:
    type: object
    properties:
      query:
        "$ref": "#/definitions/query"
  query:
    type: object
    properties:
      count:
        type: integer
        description: "The number of items (rows) returned -- specifically, the number of sub-elements in the results property"
        example: 1
      created:
        type: string
        description: "The date and time the response was created"
        example: "2017-06-14T14:30:14Z"
      lang:
        type: string
        description: "The locale for the response"
        example: "en-US"
      results:
        "$ref": "#/definitions/results"
  results:
    type: object
    properties:
      channel:
        "$ref": "#/definitions/channel"

  channel:
    type: object
    properties:
      units:
        "$ref": "#/definitions/units"
      title:
        type: string
        description: "The title of the feed, which includes the location city"
        example: "Yahoo! Weather - Singapore, South West, SG"
      link:
        type: string
        description:  "The URL for the Weather page of the forecast for this location"
        example: "http://us.rd.yahoo.com/dailynews/rss/weather/Country__Country/*https://weather.yahoo.com/country/state/city-91792352/"
      description:
        type: string
        description: "The overall description of the feed including the location"
        example: "Yahoo! Weather for Singapore, South West, SG"
      language:
        type: string
        description: "The language of the weather forecast"
        example: "en-us"
      lastBuildDate:
        type: string
        description: The last time the feed was updated. The format is in the date format defined by [RFC822 Section 5](http://www.rfc-editor.org/rfc/rfc822.txt).
        example: "Wed, 14 Jun 2017 10:30 PM SGT"
      ttl:
        type: string
        description: "Time to Live -- how long in minutes this feed should be cached."
        example: "60"
      location:
        "$ref": "#/definitions/location"
      wind:
        "$ref": "#/definitions/wind"
      atmosphere:
        "$ref": "#/definitions/atmosphere"
      astronomy:
        "$ref": "#/definitions/astronomy"
      image:
        "$ref": "#/definitions/image"
      item:
        "$ref": "#/definitions/item"
  units:
    type: object
    description: "Units for various aspects of the forecast. Note that the default is to use metric formats for the units (Celsius, kilometers, millibars, kilometers per hour)."
    properties:
      distance:
        type: string
        description: "Units for distance, mi for miles or km for kilometers"
        example: "km"
      pressure:
        type: string
        description: "Units of barometric pressure, \"in\" for pounds per square inch or \"mb\" for millibars."
        example: "mb"
      speed:
        type: string
        description: "Units of speed, \"mph\" for miles per hour or \"kph\" for kilometers per hour."
        example: "km/h"
      temperature:
        type: string
        description: "Degree units, \"f\" for Fahrenheit or \"c\" for Celsius."
        example: "C"

  location:
    type: object
    description: "The location of this forecast"
    properties:
      city:
        type: string
        description: "City name"
        example: "Singapore"
      country:
        type: string
        description: "Two-character country code"
        example: "Singapore"
      region:
        type: string
        description: "State, territory, or region, if given"
        example: "South West"

  wind:
    type: object
    description: "Forecast information about wind"
    properties:
      chill:
        type: integer
        description: "Wind chill in degrees"
        example: "81"
      direction:
        type: integer
        description: "Wind direction, in degrees"
        example: "158"
      speed:
        type: integer
        description: "Wind speed, in the units specified in the speed attribute of the units element (mph or kph)"
        example: "11.27"

  atmosphere:
    type: object
    description: "Forecast information about current atmospheric pressure, humidity, and visibility"
    properties:
      humidity:
        type: integer
        description: humidity, in percent
        example: "87"
      pressure:
        type: integer
        description: "Barometric pressure, in the units specified by the pressure attribute of the yweather:units element (\"in\" or \"mb\")."
        example: "34168.68"
      rising:
        type: integer
        description: "State of the barometric pressure: steady (0), rising (1), or falling (2)."
        example: "0"
      visibility:
        type: integer
        description: "Visibility, in the units specified by the distance attribute of the units element (\"mi\" or \"km\"). Note that the visibility is specified as the actual value * 100. For example, a visibility of 16.5 miles will be specified as 1650. A visibility of 14 kilometers will appear as 1400."
        example: "25.91"

  astronomy:
    type: object
    description: "Forecast information about current astronomical conditions"
    properties:
      sunrise:
        type: string
        description: "Today's sunrise time. The time is a string in a local time format of \"h:mm am/pm\""
        example: "6:59 am"
      sunset:
        type: string
        description: "Today's sunset time. The time is a string in a local time format of \"h:mm am/pm\""
        example: "7:11 pm"

  image:
    type: object
    description: "The image used to identify this feed"
    properties:
      title:
        type: string
        description: "The title of the image."
        example: "Yahoo! Weather"
      width:
        type: string
        description:  "The width of the image, in pixels"
        example: "142"
      height:
        type: string
        description: "The height of the image, in pixels"
        example: "18"
      link:
        type: string
        description: "Description of link"
        example: http://weather.yahoo.com
      url:
        type: string
        description: "The URL of Yahoo! Weather"
        example: http://l.yimg.com/a/i/brand/purplelogo//uh/us/news-wea.gif

  item:
    type: object
    description: "The item element contains current conditions and forecast for the given location. There are multiple weather forecast elements for today and tomorrow."
    properties:
      title:
        type: string
        description: "The forecast title and time."
        example: "Conditions for Singapore, South West, SG at 09:00 PM SGT"
      lat:
        type: string
        description: "The latitude of the location."
        example: "1.33464"
      long:
        type: string
        description: "The longitude of the location."
        example: "103.726471"
      link:
        type: string
        description: "The Yahoo! Weather URL for this forecast."
        example: "http://us.rd.yahoo.com/dailynews/rss/weather/Country__Country/*https://weather.yahoo.com/country/state/city-91792352/"
      pubDate:
        type: string
        description: The date and time this forecast was posted, in the date format defined by [RFC822 Section 5](http://www.rfc-editor.org/rfc/rfc822.txt).
        example: "Wed, 14 Jun 2017 09:00 PM SGT"
      condition:
        "$ref": "#/definitions/condition"
      forecast:
        "$ref": "#/definitions/forecast"
      description:
        type: string
        description: "A simple summary of the current conditions and tomorrow's forecast, in HTML format, including a link to Yahoo! Weather for the full forecast."
        example: "<![CDATA[<img src=\"http:\/\/l.yimg.com\/a\/i\/us\/we\/52\/4.gif\"\/>\n<BR \/>\n<b>Current Conditions:<\/b>\n<BR \/>Thunderstorms\n<BR \/>\n<BR \/>\n<b>Forecast:<\/b>\n<BR \/> Fri - Thunderstorms. High: 30Low: 25\n<BR \/> Sat - Thunderstorms. High: 28Low: 25\n<BR \/> Sun - Thunderstorms. High: 28Low: 25\n<BR \/> Mon - Thunderstorms. High: 28Low: 25\n<BR \/> Tue - Thunderstorms. High: 28Low: 25\n<BR \/>\n<BR \/>\n<a href=\"http:\/\/us.rd.yahoo.com\/dailynews\/rss\/weather\/Country__Country\/*https:\/\/weather.yahoo.com\/country\/state\/city-91792352\/\">Full Forecast at Yahoo! Weather<\/a>\n<BR \/>\n<BR \/>\n(provided by <a href=\"http:\/\/www.weather.com\" >The Weather Channel<\/a>)\n<BR \/>\n]]>"
      guid:
        "$ref": "#/definitions/guid"

  condition:
    type: object
    description: "The current weather conditions"
    properties:
      code:
        type: integer
        description: The condition code for this forecast. You could use this code to choose a text description or image for the forecast. The possible values for this element are described in the [Condition Codes](https://developer.yahoo.com/weather/).documentation.html.
        example: "33"
      date:
        type: string
        description: The current date and time for which this forecast applies. The date is in [RFC822 Section 5 format](http://www.rfc-editor.org/rfc/rfc822.txt).
        example: "Wed, 14 Jun 2017 09:00 PM SGT"
      temp:
        type: integer
        description: "The current temperature, in the units specified by the units element."
        example: "26"
      text:
        type: string
        description: "A textual description of conditions"
        example: "Mostly Clear"

  forecast:
    type: array
    items:
      "$ref": "#/definitions/forecast array"

  forecast array:
    type: object
    description: "The weather forecast for a specific day. The item element contains multiple forecast elements for today and tomorrow."
    properties:
      code:
        type: integer
        description: The condition code for this forecast. You could use this code to choose a text description or image for the forecast. The possible values for this element are described in the [Condition Codes](https://developer.yahoo.com/weather/documentation.html#codes).
        example: "4"
      date:
        type: string
        description: "The date to which this forecast applies. The date is in 'dd Mmm yyyy' format, for example '30 Nov 2005'"
        example: "14 Jun 2017"
      day:
        type: string
        description: "Day of the week to which this forecast applies. Possible values are Mon Tue Wed Thu Fri Sat Sun."
        example: "Wed"
      high:
        type: integer
        description:  "The forecasted high temperature for this day, in the units specified by the units element."
        example: "28"
      low:
        type: integer
        description: "The forecasted low temperature for this day, in the units specified by the units element."
        example: "25"
      text:
        type: string
        description: "A textual description of conditions"
        example: "Thunderstorms"

  guid:
    type: object
    description: "Unique identifier for the forecast, made up of the location ID, the date, and the time."
    properties:
      isPermaLink:
        type: string
        description: "The attribute isPermaLink is false."
        example: "false"
```

## Swagger 3.0 specification file

Smartbear recently announced [support for the 3.0 version of the OpenAPI (Swagger) spec in SwaggerHub](https://swaggerhub.com/blog/news/openapi-3-0-swaggerhub-support/). The 3.0 version of the OpenAPI spec provides a number of enhancements, such as support for callbacks, linking between operations, enhanced examples, and easier re-use). You can read about the [3.0 features here](https://swagger.io/announcing-openapi-3-0/) in more detail.

{% include tip.html content="I used [APIMATIC](https://apimatic.io/transformer) to convert the 2.0 spec to the 3.0 spec automatically. You can also use APIMATIC to transform your spec file into a number of other outputs, such as RAML, Postman, or API Blueprint. To see the difference between the 2.0 and the 3.0 code, you can copy these code samples to separate files and then use an application like Diffmerge to highlight the differences." %}

```yaml
openapi: 3.0.0
info:
  title: Weather API from Mashape
  description: "This is a sample spec that describes a Mashape Weather API as an example to demonstrate features in the Swagger-2.0 specification. This output is part of the <a href=\"http://idratherbewriting.com/learnapidoc\">Documenting REST API course</a> on my site. The Weather API displays forecast data by latitude and longitude. It's a simple weather API, but the data comes from Yahoo Weather Service. The weatherdata endpoint delivers the most robust package of information of the endpoints here.\n\nTo explore the API, you'll need an API key. You can sign up for an API through Mashape, or you can just use this one\\: `EF3g83pKnzmshgoksF83V6JB6QyTp1cGrrdjsnczTkkYgYrp8p`. For the latitude and longitude parameters, you can get this information from the URL of a location on Google Maps. For example, for Santa Clara, California, use the following\\:\n* **lat**: `37.3708698`\n* **lng**: `-122.037593` \n"
  version: 2.1
servers:
- url: https://simple-weather.p.mashape.com
  variables: {}
paths:
  /aqi:
    get:
      tags:
      - Air Quality
      summary: getAqi
      description: Gets the air quality index
      operationId: GetAqi
      parameters:
      - name: lat
        in: query
        description: latitude
        required: true
        style: form
        explode: false
        schema:
          type: string
      - name: lng
        in: query
        description: longitude
        required: true
        style: form
        explode: false
        schema:
          type: string
      responses:
        200:
          description: AQI response
          content:
            text/plain:
              schema:
                type: string
                description: AQI response
                example: 52
              example: 52
      deprecated: false
      security:
      - Mashape-Key: []
      x-operation-settings:
        CollectParameters: false
        AllowDynamicQueryParameters: false
        AllowDynamicFormParameters: false
        IsMultiContentStreaming: false
  /weather:
    get:
      tags:
      - Weather Forecast
      summary: getWeather
      description: Gets the weather forecast in abbreviated form
      operationId: GetWeather
      parameters:
      - name: lat
        in: query
        description: latitude coordinates
        required: true
        style: form
        explode: false
        schema:
          type: string
      - name: lng
        in: query
        description: longitude coordinates
        required: true
        style: form
        explode: false
        schema:
          type: string
      responses:
        200:
          description: weather response
          content:
            text/plain:
              schema:
                type: string
                description: weather response
                example: 26 c, Mostly Clear at Singapore, Singapore
              example: 26 c, Mostly Clear at Singapore, Singapore
      deprecated: false
      security:
      - Mashape-Key: []
      x-operation-settings:
        CollectParameters: false
        AllowDynamicQueryParameters: false
        AllowDynamicFormParameters: false
        IsMultiContentStreaming: false
  /weatherdata:
    get:
      tags:
      - Weather Data
      summary: getWeatherData
      description: Get weather forecast with lots of details
      operationId: GetWeatherData
      parameters:
      - name: lat
        in: query
        description: latitude
        required: true
        style: form
        explode: false
        schema:
          type: string
      - name: lng
        in: query
        description: longitude
        required: true
        style: form
        explode: false
        schema:
          type: string
      responses:
        200:
          description: Successful operation
          content:
            application/json:
              schema:
                description: Successful operation
                $ref: '#/components/schemas/WeatherdataResponse'
      deprecated: false
      security:
      - Mashape-Key: []
      x-operation-settings:
        CollectParameters: false
        AllowDynamicQueryParameters: false
        AllowDynamicFormParameters: false
        IsMultiContentStreaming: false
components:
  schemas:
    WeatherdataResponse:
      title: weatherdata response
      type: object
      properties:
        query:
          $ref: '#/components/schemas/Query'
    Query:
      title: query
      type: object
      properties:
        count:
          type: integer
          description: The number of items (rows) returned -- specifically, the number of sub-elements in the results property
          format: int32
          example: 1
        created:
          type: string
          description: The date and time the response was created
          example: 6/14/2017 2:30:14 PM
        lang:
          type: string
          description: The locale for the response
          example: en-US
        results:
          $ref: '#/components/schemas/Results'
    Results:
      title: results
      type: object
      properties:
        channel:
          $ref: '#/components/schemas/Channel'
    Channel:
      title: channel
      type: object
      properties:
        units:
          description: Units for various aspects of the forecast. Note that the default is to use metric formats for the units (Celsius, kilometers, millibars, kilometers per hour).
          $ref: '#/components/schemas/Units'
        title:
          type: string
          description: The title of the feed, which includes the location city
          example: Yahoo! Weather - Singapore, South West, SG
        link:
          type: string
          description: The URL for the Weather page of the forecast for this location
          example: http://us.rd.yahoo.com/dailynews/rss/weather/Country__Country/*https://weather.yahoo.com/country/state/city-91792352/
        description:
          type: string
          description: The overall description of the feed including the location
          example: Yahoo! Weather for Singapore, South West, SG
        language:
          type: string
          description: The language of the weather forecast
          example: en-us
        lastBuildDate:
          type: string
          description: The last time the feed was updated. The format is in the date format defined by [RFC822 Section 5](http://www.rfc-editor.org/rfc/rfc822.txt).
          example: Wed, 14 Jun 2017 10:30 PM SGT
        ttl:
          type: string
          description: Time to Live -- how long in minutes this feed should be cached.
          example: 60
        location:
          description: The location of this forecast
          $ref: '#/components/schemas/Location'
        wind:
          description: Forecast information about wind
          $ref: '#/components/schemas/Wind'
        atmosphere:
          description: Forecast information about current atmospheric pressure, humidity, and visibility
          $ref: '#/components/schemas/Atmosphere'
        astronomy:
          description: Forecast information about current astronomical conditions
          $ref: '#/components/schemas/Astronomy'
        image:
          description: The image used to identify this feed
          $ref: '#/components/schemas/Image'
        item:
          description: The item element contains current conditions and forecast for the given location. There are multiple weather forecast elements for today and tomorrow.
          $ref: '#/components/schemas/Item'
    Units:
      title: units
      type: object
      properties:
        distance:
          type: string
          description: Units for distance, mi for miles or km for kilometers
          example: km
        pressure:
          type: string
          description: Units of barometric pressure, "in" for pounds per square inch or "mb" for millibars.
          example: mb
        speed:
          type: string
          description: Units of speed, "mph" for miles per hour or "kph" for kilometers per hour.
          example: km/h
        temperature:
          type: string
          description: Degree units, "f" for Fahrenheit or "c" for Celsius.
          example: C
      description: Units for various aspects of the forecast. Note that the default is to use metric formats for the units (Celsius, kilometers, millibars, kilometers per hour).
    Location:
      title: location
      type: object
      properties:
        city:
          type: string
          description: City name
          example: Singapore
        country:
          type: string
          description: Two-character country code
          example: Singapore
        region:
          type: string
          description: State, territory, or region, if given
          example: South West
      description: The location of this forecast
    Wind:
      title: wind
      type: object
      properties:
        chill:
          type: integer
          description: Wind chill in degrees
          format: int32
          example: 81
        direction:
          type: integer
          description: Wind direction, in degrees
          format: int32
          example: 158
        speed:
          type: integer
          description: Wind speed, in the units specified in the speed attribute of the units element (mph or kph)
          format: int32
      description: Forecast information about wind
    Atmosphere:
      title: atmosphere
      type: object
      properties:
        humidity:
          type: integer
          description: humidity, in percent
          format: int32
          example: 87
        pressure:
          type: integer
          description: Barometric pressure, in the units specified by the pressure attribute of the yweather:units element ("in" or "mb").
          format: int32
        rising:
          type: integer
          description: 'State of the barometric pressure: steady (0), rising (1), or falling (2).'
          format: int32
          example: 0
        visibility:
          type: integer
          description: Visibility, in the units specified by the distance attribute of the units element ("mi" or "km"). Note that the visibility is specified as the actual value * 100. For example, a visibility of 16.5 miles will be specified as 1650. A visibility of 14 kilometers will appear as 1400.
          format: int32
      description: Forecast information about current atmospheric pressure, humidity, and visibility
    Astronomy:
      title: astronomy
      type: object
      properties:
        sunrise:
          type: string
          description: Today's sunrise time. The time is a string in a local time format of "h:mm am/pm"
          example: 6:59 am
        sunset:
          type: string
          description: Today's sunset time. The time is a string in a local time format of "h:mm am/pm"
          example: 7:11 pm
      description: Forecast information about current astronomical conditions
    Image:
      title: image
      type: object
      properties:
        title:
          type: string
          description: The title of the image.
          example: Yahoo! Weather
        width:
          type: string
          description: The width of the image, in pixels
          example: 142
        height:
          type: string
          description: The height of the image, in pixels
          example: 18
        link:
          type: string
          description: Description of link
          example: http://weather.yahoo.com
        url:
          type: string
          description: The URL of Yahoo! Weather
          example: http://l.yimg.com/a/i/brand/purplelogo//uh/us/news-wea.gif
      description: The image used to identify this feed
    Item:
      title: item
      type: object
      properties:
        title:
          type: string
          description: The forecast title and time.
          example: Conditions for Singapore, South West, SG at 09:00 PM SGT
        lat:
          type: string
          description: The latitude of the location.
          example: 1.33464
        long:
          type: string
          description: The longitude of the location.
          example: 103.726471
        link:
          type: string
          description: The Yahoo! Weather URL for this forecast.
          example: http://us.rd.yahoo.com/dailynews/rss/weather/Country__Country/*https://weather.yahoo.com/country/state/city-91792352/
        pubDate:
          type: string
          description: The date and time this forecast was posted, in the date format defined by [RFC822 Section 5](http://www.rfc-editor.org/rfc/rfc822.txt).
          example: Wed, 14 Jun 2017 09:00 PM SGT
        condition:
          description: The current weather conditions
          $ref: '#/components/schemas/Condition'
        forecast:
          type: array
          items:
            $ref: '#/components/schemas/ForecastArray'
          description: ''
        description:
          type: string
          description: A simple summary of the current conditions and tomorrow's forecast, in HTML format, including a link to Yahoo! Weather for the full forecast.
          example: "<![CDATA[<img src=\"http:\/\/l.yimg.com\/a\/i\/us\/we\/52\/4.gif\"\/>\n<BR \/>\n<b>Current Conditions:<\/b>\n<BR \/>Thunderstorms\n<BR \/>\n<BR \/>\n<b>Forecast:<\/b>\n<BR \/> Fri - Thunderstorms. High: 30Low: 25\n<BR \/> Sat - Thunderstorms. High: 28Low: 25\n<BR \/> Sun - Thunderstorms. High: 28Low: 25\n<BR \/> Mon - Thunderstorms. High: 28Low: 25\n<BR \/> Tue - Thunderstorms. High: 28Low: 25\n<BR \/>\n<BR \/>\n<a href=\"http:\/\/us.rd.yahoo.com\/dailynews\/rss\/weather\/Country__Country\/*https:\/\/weather.yahoo.com\/country\/state\/city-91792352\/\">Full Forecast at Yahoo! Weather<\/a>\n<BR \/>\n<BR \/>\n(provided by <a href=\"http:\/\/www.weather.com\" >The Weather Channel<\/a>)\n<BR \/>\n]]>"
        guid:
          type: string
          description: Unique identifier for the forecast, made up of the location ID, the date, and the time.
          format: uuid
      description: The item element contains current conditions and forecast for the given location. There are multiple weather forecast elements for today and tomorrow.
    Condition:
      title: condition
      type: object
      properties:
        code:
          type: integer
          description: The condition code for this forecast. You could use this code to choose a text description or image for the forecast. The possible values for this element are described in the [Condition Codes](https://developer.yahoo.com/weather/).documentation.html.
          format: int32
          example: 33
        date:
          type: string
          description: The current date and time for which this forecast applies. The date is in RFC822 Section 5 format.
          example: Wed, 14 Jun 2017 09:00 PM SGT
        temp:
          type: integer
          description: The current temperature, in the units specified by the units element.
          format: int32
          example: 26
        text:
          type: string
          description: A textual description of conditions
          example: Mostly Clear
      description: The current weather conditions
    ForecastArray:
      title: forecast array
      type: object
      properties:
        code:
          type: integer
          description: The condition code for this forecast. You could use this code to choose a text description or image for the forecast. The possible values for this element are described in the [Condition Codes](https://developer.yahoo.com/weather/documentation.html#codes).
          format: int32
          example: 4
        date:
          type: string
          description: The date to which this forecast applies. The date is in 'dd Mmm yyyy' format, for example '30 Nov 2005'
          example: 14 Jun 2017
        day:
          type: string
          description: Day of the week to which this forecast applies. Possible values are Mon Tue Wed Thu Fri Sat Sun.
          example: Wed
        high:
          type: integer
          description: The forecasted high temperature for this day, in the units specified by the units element.
          format: int32
          example: 28
        low:
          type: integer
          description: The forecasted low temperature for this day, in the units specified by the units element.
          format: int32
          example: 25
        text:
          type: string
          description: A textual description of conditions
          example: Thunderstorms
      description: The weather forecast for a specific day. The item element contains multiple forecast elements for today and tomorrow.
    Guid:
      title: guid
      type: object
      properties:
        isPermaLink:
          type: string
          description: The attribute isPermaLink is false.
          example: false
      description: Unique identifier for the forecast, made up of the location ID, the date, and the time.
  securitySchemes:
    Mashape-Key:
      type: apiKey
      description: ''
      name: X-Mashape-Key
      in: header
security:
- Mashape-Key: []
```

{% include random_ad.html %}
