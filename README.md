# LLM-vaidation
Pydantic validation of LLM response

#Web Scraping and Structured Data Extraction
This project utilizes prompt engineering and Pydantic validation to extract structured data from a specified URL using large language models. The process is outlined as follows:

###Step 1: Data Extraction
To extract data from a provided URL, we leverage BeautifulSoup and an HTML parser for web page scraping.

###Step 2: Define Pydantic Schema
Once the data is obtained, we define a Pydantic schema that represents the entities to be extracted. This schema serves as a blueprint for the expected structure of the output.
For the given use case, schema used is
```
class WebScrape1(BaseModel):
    date: str
    film: str
    gross: int

```

###Step 3: Model Execution
The Pydantic schema is passed to the language model as a prompt, along with instructions to generate output in JSON format. If the model fails to produce JSON output, an error handling function retries the process, incorporating the error message.

This retry mechanism persists until the desired structured output is obtained, ensuring robustness in the face of potential model failures.

```
def handle_validation_failure(error, prompt, num_tries):
  if num_tries > 3:
    return "Couldnt validate LLM response"
  else:
    out = askLLM(error + prompt+ "Make sure the output is json")
    print(out)
    return out

```

###Step 4: Pydantic Validation
Once the output is generated in a JSON-compatible format, it undergoes validation using Pydantic. This validation ensures that the JSON data adheres to the specified schema, eliminating missing data or incorrect data types.


For the given example the model initially produced results that were not parsable and did not follow the structure provided:
```
[
{
"properties": {
"date": {
"title": "Date",
"type": "string"
},
"film": {
"title": "Film",
"type": "string"
},
"gross": {
"title": "Gross",
"type": "integer"
}
},
"required": [
"date",
"film",
"gross"
],
"type": "object",
"title": "WebScrape1",
"data": [
{
"date": "January 8, 2023",
"film": "Avatar: The Way of Water",
"gross": 45838986
},
{
"date": "January 15, 2023",
"film": "Black Panther: Wakanda Forever",
"gross": 32824684
},
.
.
.
```

On subsequent iterations, it is successfully able to generate a response which is in JSON format:

```
[
{"date": "January 8, 2023", "film": "Avatar: The Way of Water", "gross": 45838986},
{"date": "January 15, 2023", "film": "Black Panther: Wakanda Forever", "gross": 32824684},
{"date": "January 22, 2023", "film": "Avatar: The Way of Water", "gross": 20133106},
{"date": "January 29, 2023", "film": "Avatar: The Way of Water", "gross": 15968532},
{"date": "February 5, 2023", "film": "Knock at the Cabin", "gross": 14127170},
{"date": "February 12, 2023", "film": "Magic Mike's Last Dance", "gross": 8305317},
{"date": "February 19, 2023", "film": "Ant-Man and the Wasp: Quantumania", "gross": 106109650},
{"date": "February 26, 2023", "film": null, "gross": 31964803},
{"date": "March 5, 2023", "film": "Creed III", "gross": 58370007},
{"date": "March 12, 2023", "film": "Scream VI", "gross": 44447270},
{"date": "March 19, 2023", "film": "Shazam! Fury of the Gods", "gross": 30111158},
{"date": "March 26, 2023", "film": "John Wick: Chapter 4", "gross": 73817950},
{"date": "April 2, 2023", "film": "Dungeons & Dragons: Honor Among Thieves", "gross": 37205784},
{"date": "April 9, 2023", "film": "The Super Mario Bros. Movie", "gross": 146361865},
{"date": "April 16, 2023", "film": "The Super Mario Bros. Movie", "gross": 92347190},
{"date": "April 23, 2023", "film": "The Super Mario Bros. Movie", "gross": 59930940},
{"date": "April 30, 2023", "film": "The Super Mario Bros. Movie", "gross": 40835805},
{"date": "May 7, 2023", "film": "Guardians of the Galaxy Vol. 3", "gross": 118414021},
{"date": "May 14, 2023", "film": null, "gross": 62008548},
{"date": "May 21, 2023", "film": "Fast X", "gross": 67017410},
{"date": "May 28, 2023", "film": "The Little Mermaid", "gross": 95578040},
{"date": "June 4, 2023", "film": "Spider-Man: Across the Spider-Verse", "gross": 120663589},
{"date": "June 11, 2023", "film": "Transformers: Rise of the Beasts", "gross": 61045464},
{"date": "June 18, 2023", "film": "The Flash", "gross": 55043679},
{"date": "June 25, 2023", "film": "Spider-Man: Across the Spider-Verse", "gross": 19003633},
{"date": "July 2, 2023", "film": "Indiana Jones and the Dial of Destiny", "gross": 60368101},
{"date": "July 9, 2023", "film": "Insidious: The Red Door", "gross": 33013036},
{"date": "July 16, 2023", "film": "Mission: Impossible – Dead Reckoning Part One", "gross": 54688347},
{"date": "July 23, 2023", "film": "Barbie", "gross": 162022044},
{"date": "July 30, 2023", "film": "Barbie", "gross": 93011602},
{"date": "August 6, 2023", "film": null, "gross": 53008647},
{"date": "August 13, 2023", "film": "Barbie", "gross": 33833294},
{"date": "August 20, 2023", "film": "Blue Beetle", "gross": 25030225},
{"date": "August 27, 2023", "film": "Gran Turismo", "gross": 17410552},
{"date": "September 3, 2023", "film": "The Equalizer 3", "gross": 34604229},
{"date": "September 10, 2023", "film": "The Nun II", "gross": 32603336},
{"date": "September 17, 2023", "film": null, "gross": 14534579},
{"date": "September 24, 2023", "film": null, "gross": 8550110},
{"date": "October 1, 2023", "film": "PAW Patrol: The Mighty Movie", "gross": 22764354},
{"date": "October 8, 2023", "film": "The Exorcist: Believer", "gross": 26497600},
{"date": "October 15, 2023", "film": "Taylor Swift: The Eras Tour", "gross": 92347190},
{"date": "October 22, 2023", "film": "Taylor Swift: The Eras Tour", "gross": 44447270},
{"date": "October 29, 2023", "film": "Five Nights at Freddy's", "gross": 80001720},
{"date": "November 5, 2023", "film": null, "gross": 37205784},
{"date": "November 12, 2023", "film": "The Marvels", "gross": 46110859},
{"date": "November 19, 2023", "film": "The Hunger Games: The Ballad of Songbirds & Snakes", "gross": 44607143},
{"date": "November 26, 2023", "film": "The Super Mario Bros. Movie", "gross": 29042517},
{"date": "December 3, 2023", "film": "Wonka", "gross": 120663589},
{"date": "December 10, 2023", "film": "The Boy and the Heron", "gross": 13011722},
{"date": "December 17, 2023", "film": "Indiana Jones and the Dial of Destiny", "gross": 60368101},
{"date": "December 24, 2023", "film": "The Flash", "gross": 55043679}
]
```
But this data still contains some missing fields. These can be identified using pydantic validation.

Validated data:
```
{'date': 'January 8, 2023', 'film': 'Avatar: The Way of Water', 'gross': 45838986}
{'date': 'January 15, 2023', 'film': 'Black Panther: Wakanda Forever', 'gross': 32824684}
{'date': 'January 22, 2023', 'film': 'Avatar: The Way of Water', 'gross': 20133106}
{'date': 'January 29, 2023', 'film': 'Avatar: The Way of Water', 'gross': 15968532}
{'date': 'February 5, 2023', 'film': 'Knock at the Cabin', 'gross': 14127170}
{'date': 'February 12, 2023', 'film': "Magic Mike's Last Dance", 'gross': 8305317}
{'date': 'February 19, 2023', 'film': 'Ant-Man and the Wasp: Quantumania', 'gross': 106109650}
{'date': 'March 5, 2023', 'film': 'Creed III', 'gross': 58370007}
{'date': 'March 12, 2023', 'film': 'Scream VI', 'gross': 44447270}
{'date': 'March 19, 2023', 'film': 'Shazam! Fury of the Gods', 'gross': 30111158}
{'date': 'March 26, 2023', 'film': 'John Wick: Chapter 4', 'gross': 73817950}
{'date': 'April 2, 2023', 'film': 'Dungeons & Dragons: Honor Among Thieves', 'gross': 37205784}
{'date': 'April 9, 2023', 'film': 'The Super Mario Bros. Movie', 'gross': 146361865}
{'date': 'April 16, 2023', 'film': 'The Super Mario Bros. Movie', 'gross': 92347190}
{'date': 'April 23, 2023', 'film': 'The Super Mario Bros. Movie', 'gross': 59930940}
{'date': 'April 30, 2023', 'film': 'The Super Mario Bros. Movie', 'gross': 40835805}
{'date': 'May 7, 2023', 'film': 'Guardians of the Galaxy Vol. 3', 'gross': 118414021}
{'date': 'May 21, 2023', 'film': 'Fast X', 'gross': 67017410}
{'date': 'May 28, 2023', 'film': 'The Little Mermaid', 'gross': 95578040}
{'date': 'June 4, 2023', 'film': 'Spider-Man: Across the Spider-Verse', 'gross': 120663589}
{'date': 'June 11, 2023', 'film': 'Transformers: Rise of the Beasts', 'gross': 61045464}
{'date': 'June 18, 2023', 'film': 'The Flash', 'gross': 55043679}
{'date': 'June 25, 2023', 'film': 'Spider-Man: Across the Spider-Verse', 'gross': 19003633}
{'date': 'July 2, 2023', 'film': 'Indiana Jones and the Dial of Destiny', 'gross': 60368101}
{'date': 'July 9, 2023', 'film': 'Insidious: The Red Door', 'gross': 33013036}
{'date': 'July 16, 2023', 'film': 'Mission: Impossible – Dead Reckoning Part One', 'gross': 54688347}
{'date': 'July 23, 2023', 'film': 'Barbie', 'gross': 162022044}
{'date': 'July 30, 2023', 'film': 'Barbie', 'gross': 93011602}
{'date': 'August 13, 2023', 'film': 'Barbie', 'gross': 33833294}
{'date': 'August 20, 2023', 'film': 'Blue Beetle', 'gross': 25030225}
{'date': 'August 27, 2023', 'film': 'Gran Turismo', 'gross': 17410552}
{'date': 'September 3, 2023', 'film': 'The Equalizer 3', 'gross': 34604229}
{'date': 'September 10, 2023', 'film': 'The Nun II', 'gross': 32603336}
{'date': 'October 1, 2023', 'film': 'PAW Patrol: The Mighty Movie', 'gross': 22764354}
{'date': 'October 8, 2023', 'film': 'The Exorcist: Believer', 'gross': 26497600}
{'date': 'October 15, 2023', 'film': 'Taylor Swift: The Eras Tour', 'gross': 92347190}
{'date': 'October 22, 2023', 'film': 'Taylor Swift: The Eras Tour', 'gross': 44447270}
{'date': 'October 29, 2023', 'film': "Five Nights at Freddy's", 'gross': 80001720}
{'date': 'November 12, 2023', 'film': 'The Marvels', 'gross': 46110859}
{'date': 'November 19, 2023', 'film': 'The Hunger Games: The Ballad of Songbirds & Snakes', 'gross': 44607143}
{'date': 'November 26, 2023', 'film': 'The Super Mario Bros. Movie', 'gross': 29042517}
{'date': 'December 3, 2023', 'film': 'Wonka', 'gross': 120663589}
{'date': 'December 10, 2023', 'film': 'The Boy and the Heron', 'gross': 13011722}
{'date': 'December 17, 2023', 'film': 'Indiana Jones and the Dial of Destiny', 'gross': 60368101}
{'date': 'December 24, 2023', 'film': 'The Flash', 'gross': 55043679}

```

Rejected data:

```
{'date': 'February 26, 2023', 'film': None, 'gross': 31964803}
{'date': 'May 14, 2023', 'film': None, 'gross': 62008548}
{'date': 'August 6, 2023', 'film': None, 'gross': 53008647}
{'date': 'September 17, 2023', 'film': None, 'gross': 14534579}
{'date': 'September 24, 2023', 'film': None, 'gross': 8550110}
{'date': 'November 5, 2023', 'film': None, 'gross': 37205784}

```
There are several ways to handle this data, It can either be sent back to the model with prompt related to that particular entry or can be handled manually

Carrying out operations on the data:
Since the data is now in a form that is parsable, several actions such as searching and sorting can now be performed

Sorting the data based on gross revenue

```
sorted_data = sorted(validated, key=lambda x: x['gross'], reverse=True)
```

output:
```
{
   "date":"July 23, 2023",
   "film":"Barbie",
   "gross":162022044
},
{
   "date":"April 9, 2023",
   "film":"The Super Mario Bros. Movie",
   "gross":146361865
},
{
   "date":"October 1, 2023",
   "film":"The Super Mario Bros. Movie",
   "gross":146361865
},
{
   "date":"November 26, 2023",
   "film":"The Super Mario Bros. Movie",
   "gross":146361865
},
{
   "date":"June 4, 2023",
   "film":"Spider-Man: Across the Spider-Verse",
   "gross":120663589
},
{
   "date":"May 7, 2023",
   "film":"Guardians of the Galaxy Vol. 3",
   "gross":118414021
},
{
   "date":"February 19, 2023",
   "film":"Ant-Man and the Wasp: Quantumania",
   "gross":106109650
},
{
   "date":"May 28, 2023",
   "film":"The Little Mermaid",
   "gross":95578040
},
{
   "date":"July 30, 2023",
   "film":"Barbie",
   "gross":93011602
},
{
   "date":"April 16, 2023",
   "film":"The Super Mario Bros. Movie",
   "gross":92347190
},
{
   "date":"October 8, 2023",
   "film":"The Super Mario Bros. Movie",
   "gross":92347190
},
{
   "date":"October 29, 2023",
   "film":"Five Nights at Freddy's",
   "gross":80001780
},
{
   "date":"March 26, 2023",
   "film":"John Wick: Chapter 4",
   "gross":73817950
},
{
   "date":"May 21, 2023",
   "film":"Fast X",
   "gross":67017410
},
{
   "date":"June 11, 2023",
   "film":"Transformers: Rise of the Beasts",
   "gross":61045464
},
{
   "date":"July 2, 2023",
   "film":"Indiana Jones and the Dial of Destiny",
   "gross":60368101
},
{
   "date":"April 23, 2023",
   "film":"The Super Mario Bros. Movie",
   "gross":59930940
},
{
   "date":"October 15, 2023",
   "film":"The Super Mario Bros. Movie",
   "gross":59930940
},
{
   "date":"October 22, 2023",
   "film":"The Super Mario Bros. Movie",
   "gross":59930940
},
{
   "date":"March 5, 2023",
   "film":"Creed III",
   "gross":58370007
},
{
   "date":"June 18, 2023",
   "film":"The Flash",
   "gross":55043679
},
{
   "date":"December 17, 2023",
   "film":"The Flash",
   "gross":55043679
},
{
   "date":"July 16, 2023",
   "film":"Mission: Impossible – Dead Reckoning Part One",
   "gross":54688347
},
{
   "date":"November 12, 2023",
   "film":"The Marvels",
   "gross":46110859
},
{
   "date":"January 8, 2023",
   "film":"Avatar: The Way of Water",
   "gross":45838986
},
{
   "date":"November 19, 2023",
   "film":"The Hunger Games: The Ballad of Songbirds & Snakes",
   "gross":44607143
},
{
   "date":"March 12, 2023",
   "film":"Scream VI",
   "gross":44447270
},
{
   "date":"April 30, 2023",
   "film":"The Super Mario Bros. Movie",
   "gross":40835805
},
{
   "date":"December 3, 2023",
   "film":"Wonka",
   "gross":39000000
},
{
   "date":"April 2, 2023",
   "film":"Dungeons & Dragons: Honor Among Thieves",
   "gross":37205784
},
{
   "date":"September 24, 2023",
   "film":"Dungeons & Dragons: Honor Among Thieves",
   "gross":37205784
},
{
   "date":"November 5, 2023",
   "film":"Dungeons & Dragons: Honor Among Thieves",
   "gross":37205784
},
{
   "date":"September 3, 2023",
   "film":"The Equalizer 3",
   "gross":34604229
},
{
   "date":"August 13, 2023",
   "film":"Barbie",
   "gross":33833294
},
{
   "date":"July 9, 2023",
   "film":"Insidious: The Red Door",
   "gross":33013036
},
{
   "date":"January 15, 2023",
   "film":"Black Panther: Wakanda Forever",
   "gross":32824684
},
{
   "date":"September 10, 2023",
   "film":"The Nun II",
   "gross":32603336
},
{
   "date":"March 19, 2023",
   "film":"Shazam! Fury of the Gods",
   "gross":30111158
},
{
   "date":"August 20, 2023",
   "film":"Blue Beetle",
   "gross":25030225
},
{
   "date":"January 22, 2023",
   "film":"Avatar: The Way of Water",
   "gross":20133106
},
{
   "date":"December 24, 2023",
   "film":"The Fabelmans",
   "gross":20000000
},
{
   "date":"June 25, 2023",
   "film":"Spider-Man: Across the Spider-Verse",
   "gross":19003633
},
{
   "date":"August 27, 2023",
   "film":"Gran Turismo",
   "gross":17410552
},
{
   "date":"January 29, 2023",
   "film":"Avatar: The Way of Water",
   "gross":15968532
},
{
   "date":"February 5, 2023",
   "film":"Knock at the Cabin",
   "gross":14127170
},
{
   "date":"December 10, 2023",
   "film":"The Boy and the Heron",
   "gross":13011722
},
{
   "date":"February 12, 2023",
   "film":"Magic Mike's Last Dance",
   "gross":8305317
}
```

