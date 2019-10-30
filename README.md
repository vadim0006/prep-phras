# pphrase
pphrase is a tool which extracts and (in the close future) semantically labels prepositional phrases in any given text.
For now only extraction is available.

It is a language agnostic tool but is primarily designed for slavic languages.

pphrase is capable of working with prepositions which are parts of funсtional words and derivative prepositions (such dictionaries are available for Russian and Belarusian only for now but can be manually provided as a list as well).

It is important to mention that the prepositional phrases here are considered to be a trinary union of the preposition, its host and the dependent (as in "jump over dog").

  - Initialize pphrase extractor with model and lists of functional words and derivatives (if needed)
  - Start phrases extraction
  - Receive a list of dictionaries of the following format:
    
    ```python
    [{'phrase': 'prepositional_phrase (eg. jumped over the lazy dog)', 
      'host': 'host (eg. jumped)',
      'prep': 'preposition (eg. over)',
      'dependant': 'dependant (eg.dog)',
       ...
       'other tags and lemmas if tag = True']
    ```


## Installation

Apart from the dependencies pphrase requires a [UDPipe model](https://ufal.mff.cuni.cz/udpipe/models) to run. Get one before initializing the extractor!

```sh
$ pip install pphrase
```

#### Example for English
```
import pphrase

text = 'the quick brown fox jumped over the lazy dog'

udpipe_model='models/english-ewt-ud-2.4-190531.udpipe'

ex = pphrase.Extractor(udpipe_model=udpipe_model, lang='en', tag=False)
ex.extract_phrases(text)

>>>   [{'phrase': 'jumped over the lazy dog',
        'host': 'jumped',
        'prep': 'over',
        'dependant': 'dog'}]
``` 

#### Example for Russian 
```
import pphrase

text = 'В одной из отдаленных улиц Москвы, в сером доме с белыми колоннами, \
        антресолью и покривившимся балконом, жила некогда барыня, вдова, \
        окруженная многочисленною дворней. Сыновья ее служили в Петербурге, \
        дочери вышли замуж; она выезжала редко и уединенно доживала последние \
        годы своей скупой и скучающей старости. День ее, нерадостный и ненастный, \
        давно прошел; но и вечер ее был чернее ночи.'

udpipe_model='models/russian-syntagrus-ud-2.4-190531.udpipe'

ex = pphrase.Extractor(udpipe_model=udpipe_model, lang='ru', tag=True)
ex.extract_phrases(text)

>>>   [{'phrase': 'В одной из отдаленных улиц Москвы жила',
        'host': 'жила',
        'prep': 'В одной из',
        'dependant': 'улиц',
        'host_pos': 'VERB',
        'host_lemma': 'жить',
        'dependant_case': 'gent',
        'dependant_pos': 'NOUN',
        'dependant_num': 'plur',
        'dependant_lemma': 'улица'},
       {'phrase': 'служили в Петербурге',
        'host': 'служили',
        'prep': 'в',
        'dependant': 'Петербурге',
        'host_pos': 'VERB',
        'host_lemma': 'служить',
        'dependant_case': 'loct',
        'dependant_pos': 'NOUN',
        'dependant_num': 'sing',
        'dependant_lemma': 'петербург'}]
```

#### Todos

 - Add semantic processing
