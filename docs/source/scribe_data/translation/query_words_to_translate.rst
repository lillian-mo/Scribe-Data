query_words_to_translate
========================

`View code on Github <https://github.com/scribe-org/Scribe-Data/tree/main/src/scribe_data/translation/query_words_to_translate.sparql>`_

Queries words in a given language to be translated to target languages. Enter this query at https://query.wikidata.org/.

Currently translations (P5972, Q7553) are not widely used on Wikidata. An initial goal is that Scribe will directly query these features. This would allow for basic translations from any system or chosen language. Words with multiple translations could also prompt the keyboard to be replaced with the options. For now words are querried and machine translated using 🤗 Transformers.

.. code:: sparql

    # tool: scribe-data
    SELECT
        ?word

    WHERE {
        # We want nouns, proper nouns, pronouns, personal pronouns, verbs, adjectives, adverbs, prepositions, postpositions, conjunctions and articles.
        VALUES ?wordCategories {
            wd:Q1084 wd:Q147276 wd:Q36224 wd:Q468801 wd:Q24905 wd:Q34698 wd:Q380057 wd:Q4833830 wd:Q161873 wd:Q191536 wd:Q103184
        }

        ?lexeme dct:language wd:LANGUAGE_QID; # replace language qid here ;
            wikibase:lexicalCategory ?category ;
            wikibase:lemma ?lemma .

        FILTER (?category = ?wordCategories)

        SERVICE wikibase:label {
            bd:serviceParam wikibase:language "[AUTO_LANGUAGE]".
            ?lemma rdfs:label ?word .
        }
    }

..
