---
layout: doc_page
---

## Theta Sketch Pig UDFs

### Instructions

* get jars
* save the following script as theta.pig
* adjust jar versions and paths if necessary
* save the below data into a file called "data.txt"
* copy data to hdfs: "hdfs dfs -copyFromLocal data.txt"
* run pig script: "pig theta.pig"

### theta.pig script

    register sketches-core-0.5.2.jar;
    register sketches-pig-0.5.2.jar;

    define dataToSketch com.yahoo.sketches.pig.theta.DataToSketch('32');
    define mergeSketch com.yahoo.sketches.pig.theta.Merge('32');
    define getResult com.yahoo.sketches.pig.theta.Estimate();

    a = load 'data.txt' as (id, category);
    b = group a by category;
    c = foreach b generate flatten(group) as (category), flatten(dataToSketch(a.id)) as (sketch);
    -- Sketches can be stored at this point in binary format to be used later:
    -- store c into 'intermediate/$date' using BinStorage();
    -- The next two lines print the results in human readable form for the purpose of this example
    d = foreach c generate category, getResult(sketch);
    dump d;

    -- This can be a separate query
    -- For example, the first part can produce a daily intermediate feed and store it,
    -- and this part can load several instances of this daily intermediate feed and merge them
    -- c = load 'intermediate/$date1,intermediate/$date2' using BinStorage() as (category, sketch);
    e = group c all;
    f = foreach e generate flatten(mergeSketch(c.sketch)) as (sketch);
    g = foreach f generate getResult(sketch);
    dump g;

The example input data has 2 fields: id and category.
There are 2 categories 'a' and 'b' with 50 unique IDs in each.
Most of the IDs in these categories overlap, so that there are 60 unique IDs in total.

Results:
From 'dump d':

    (a,46.91487058420659)
    (b,46.23988568048073)

From 'dump g' (merged across categories):

    (50.415577215639736)

The expected exact result would be (60.0). The estimate has high relative error because the sketch was configured with only 32 nominal entries.

### [data.txt](data.txt) (tab separated)
    01	a
    02	a
    03	a
    04	a
    05	a
    06	a
    07	a
    08	a
    09	a
    10	a
    11	a
    12	a
    13	a
    14	a
    15	a
    16	a
    17	a
    18	a
    19	a
    20	a
    21	a
    22	a
    23	a
    24	a
    25	a
    26	a
    27	a
    28	a
    29	a
    30	a
    31	a
    32	a
    33	a
    34	a
    35	a
    36	a
    37	a
    38	a
    39	a
    40	a
    41	a
    42	a
    43	a
    44	a
    45	a
    46	a
    47	a
    48	a
    49	a
    50	a
    11	b
    12	b
    13	b
    14	b
    15	b
    16	b
    17	b
    18	b
    19	b
    20	b
    21	b
    22	b
    23	b
    24	b
    25	b
    26	b
    27	b
    28	b
    29	b
    30	b
    31	b
    32	b
    33	b
    34	b
    35	b
    36	b
    37	b
    38	b
    39	a
    40	a
    41	a
    42	a
    43	a
    44	a
    45	a
    46	a
    47	a
    48	a
    49	a
    50	a
    11	b
    12	b
    13	b
    14	b
    15	b
    16	b
    17	b
    18	b
    19	b
    20	b
    21	b
    22	b
    23	b
    24	b
    25	b
    26	b
    27	b
    28	b
    29	b
    30	b
    31	b
    32	b
    33	b
    34	b
    35	b
    36	b
    37	b
    38	b
    39	b
    40	b
    41	b
    42	b
    43	b
    44	b
    45	b
    46	b
    47	b
    48	b
    49	b
    50	b
    51	b
    52	b
    53	b
    54	b
    55	b
    56	b
    57	b
    58	b
    59	b
    60	b