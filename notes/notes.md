# Notes
Created GitHub repo

Started with `go mod init github.com/Cosiamo/GoogleScrapper`

In `main.go` I'm importing the following packages:
```go
import (
	"fmt"

	"math/rand"
	// for http requests
	"net/http"
	"net/url"
	"strings"
	"time"

	// help with scrapping from google
	"github.com/PuerkitoBio/goquery"
)
```

Because I'm using an external package I needed to install it: `go get "github.com/PuerkitoBio/goquery"`

- countryCode is like .jp for Japan, .fr for France, .uk for England, .com for global, etc...
	- using countryCode to search in GoogleDomains
```go
// list of all available Google domains
var googleDomains = map[string]string {
	// using "search?q=" to start a search query
	// appending a query at the end of it depending on the countryCode

	// global
	"com":"https://www.google.com/search?q=",
	// Zimbabwe
	"za":"https://www.google.co.za/search?q="
}
```

- fmt.Sprintf is used to format a string

- languageCode is en for English, fr for French, es for Spanish, etc...

- "%s%s&num=%d&hl=%s&start=%d&filter=0"
	- first %s is `googleBase`
	- second %%s is `searchTerm`
	- num=%d is `count` (how many results do you want back)
	- hl=%s is `languageCode`
	- start=%d is `start` (in the for loop in buildGoogleUrls, `start` is `i` multiplied by `count`)

- `proxyString` is there if you want the server or computer to make a proxy request to Google

### GoogleScrape function
- `results` is the slice of `SearchResults`
- since the for loop is going on, we'll receive `SearchResults` one by one from passing that result to `googleResultParsing`
#### Order
- make a request to `googlePages` (the Google URL) 
- receive via `scrapeClientRequest`
- parse it (from the structure of `SearchResult`) with `googleResultParsing`
- then the `data` we receive from `googleResultParsing`, we'll range over it and append that to our `results` slice


# func googleResultParsing
- When we received some response from the request to Google, the document format can query the document to run find functions, string functions, trimming functions, etc... so we can run on a document
	- that's why we convert it into a document
- In that document we want to find "div.g" because that will contain every single request



# Country Codes
```go
var googleDomains = map[string]string {
	// using "search?q=" to start a search query
	// appending a query at the end of it depending on the countryCode

	// global - same domain as "us"
	"com":"https://www.google.com/search?q=",
	// Ascension Island - Saint Helena uses "sh"
	"ac": "https://www.google.ac/search?q=",
	// Andorra
    "ad": "https://www.google.ad/search?q=",
	// United Arab Emirates
    "ae": "https://www.google.ae/search?q=",
	// Afghanistan
    "af": "https://www.google.com.af/search?q=",
	// Antigua and Barbuda
    "ag": "https://www.google.com.ag/search?q=",
	// Anguilla
    "ai": "https://www.google.com.ai/search?q=",
	// Albania
    "al": "https://www.google.al/search?q=",
	// Armenia
    "am": "https://www.google.am/search?q=",
	// Angola
    "ao": "https://www.google.co.ao/search?q=",
	// Argentina
    "ar": "https://www.google.com.ar/search?q=",
	// American Samoa
    "as": "https://www.google.as/search?q=",
	// Austria
    "at": "https://www.google.at/search?q=",
	// Australia
    "au": "https://www.google.com.au/search?q=",
	// Azerbaijan
    "az": "https://www.google.az/search?q=",
	// Bosnia and Herzegovina
    "ba": "https://www.google.ba/search?q=",
	// Bangladesh
    "bd": "https://www.google.com.bd/search?q=",
	// Belgium
    "be": "https://www.google.be/search?q=",
	// Burkina Faso
    "bf": "https://www.google.bf/search?q=",
	// Bulgaria
    "bg": "https://www.google.bg/search?q=",
	// Bahrain
    "bh": "https://www.google.com.bh/search?q=",
	// Burundi
    "bi": "https://www.google.bi/search?q=",
	// Benin
    "bj": "https://www.google.bj/search?q=",
	// Brunei Darussalam
    "bn": "https://www.google.com.bn/search?q=",
	// Bolivia
    "bo": "https://www.google.com.bo/search?q=",
	// Brazil
    "br": "https://www.google.com.br/search?q=",
	// The Bahamas
    "bs": "https://www.google.bs/search?q=",
	// Bhutan
    "bt": "https://www.google.bt/search?q=",
	// Botswana
    "bw": "https://www.google.co.bw/search?q=",
	// Belarus
    "by": "https://www.google.by/search?q=",
	// Belize
    "bz": "https://www.google.com.bz/search?q=",
	// Canada
    "ca": "https://www.google.ca/search?q=",
	// Cambodia
    "kh": "https://www.google.com.kh/search?q=",
	// The Cocos (Keeling) Islands
    "cc": "https://www.google.cc/search?q=",
	// The Democratic Republic of the Congo
    "cd": "https://www.google.cd/search?q=",
	// Central African Republic
    "cf": "https://www.google.cf/search?q=",
	// Catalan countries
    "cat": "https://www.google.cat/search?q=",
	// Congo
    "cg": "https://www.google.cg/search?q=",
	// Switzerland
    "ch": "https://www.google.ch/search?q=",
	// Côte d'Ivoire
    "ci": "https://www.google.ci/search?q=",
	// The Cook Islands
    "ck": "https://www.google.co.ck/search?q=",
	// Chile
    "cl": "https://www.google.cl/search?q=",
	// Cameroon
    "cm": "https://www.google.cm/search?q=",
	// Colombia
    "co": "https://www.google.com.co/search?q=",
	// Costa Rica
    "cr": "https://www.google.co.cr/search?q=",
	// Cuba
    "cu": "https://www.google.com.cu/search?q=",
	// Cabo Verde
    "cv": "https://www.google.cv/search?q=",
	// Cyprus
    "cy": "https://www.google.com.cy/search?q=",
	// Czechia
    "cz": "https://www.google.cz/search?q=",
	// Germany
    "de": "https://www.google.de/search?q=",
	// Djibouti
    "dj": "https://www.google.dj/search?q=",
	// Denmark
    "dk": "https://www.google.dk/search?q=",
	// Dominica
    "dm": "https://www.google.dm/search?q=",
	// The Dominican Republic
    "do": "https://www.google.com.do/search?q=",
	// Algeria
    "dz": "https://www.google.dz/search?q=",
	// Ecuador
    "ec": "https://www.google.com.ec/search?q=",
	// Estonia
    "ee": "https://www.google.ee/search?q=",
	// Egypt
    "eg": "https://www.google.com.eg/search?q=",
	// Spain
    "es": "https://www.google.es/search?q=",
	// Ethiopia
    "et": "https://www.google.com.et/search?q=",
	// Finland
    "fi": "https://www.google.fi/search?q=",
	// Fiji
    "fj": "https://www.google.com.fj/search?q=",
	// Federated States of Micronesia
    "fm": "https://www.google.fm/search?q=",
	// France
    "fr": "https://www.google.fr/search?q=",
	// Gabon
    "ga": "https://www.google.ga/search?q=",
	// Georgia
    "ge": "https://www.google.ge/search?q=",
	// French Guiana
    "gf": "https://www.google.gf/search?q=",
	// Guernsey
    "gg": "https://www.google.gg/search?q=",
	// Ghana
    "gh": "https://www.google.com.gh/search?q=",
	// Gibraltar
    "gi": "https://www.google.com.gi/search?q=",
	// Greenland
    "gl": "https://www.google.gl/search?q=",
	// Gambia
    "gm": "https://www.google.gm/search?q=",
	// Guadeloupe
    "gp": "https://www.google.gp/search?q=",
	// Greece
    "gr": "https://www.google.gr/search?q=",
	// Guatemala
    "gt": "https://www.google.com.gt/search?q=",
	// Guyana
    "gy": "https://www.google.gy/search?q=",
	// Hong Kong
    "hk": "https://www.google.com.hk/search?q=",
	// Honduras
    "hn": "https://www.google.hn/search?q=",
	// Croatia
    "hr": "https://www.google.hr/search?q=",
	// Haiti
    "ht": "https://www.google.ht/search?q=",
	// Hungary
    "hu": "https://www.google.hu/search?q=",
	// Indonesia
    "id": "https://www.google.co.id/search?q=",
	// Iraq
    "iq": "https://www.google.iq/search?q=",
	// Ireland
    "ie": "https://www.google.ie/search?q=",
	// Israel
    "il": "https://www.google.co.il/search?q=",
	// Isle of Man
    "im": "https://www.google.im/search?q=",
	// India
    "in": "https://www.google.co.in/search?q=",
	// British Indian Ocean Territory
    "io": "https://www.google.io/search?q=",
	// Iceland
    "is": "https://www.google.is/search?q=",
	// Italy
    "it": "https://www.google.it/search?q=",
	// Jersey
    "je": "https://www.google.je/search?q=",
	// Jamaica
    "jm": "https://www.google.com.jm/search?q=",
	// Jordan
    "jo": "https://www.google.jo/search?q=",
	// Japan
    "jp": "https://www.google.co.jp/search?q=",
	// Kenya
    "ke": "https://www.google.co.ke/search?q=",
	// Kiribati
    "ki": "https://www.google.ki/search?q=",
	// Kyrgyzstan
    "kg": "https://www.google.kg/search?q=",
	// South Korea
    "kr": "https://www.google.co.kr/search?q=",
	// Kuwait
    "kw": "https://www.google.com.kw/search?q=",
	// Kazakhstan
    "kz": "https://www.google.kz/search?q=",
	// The Lao People's Democratic Republic
    "la": "https://www.google.la/search?q=",
	// Lebanon
    "lb": "https://www.google.com.lb/search?q=",
	// Saint Lucia
    "lc": "https://www.google.com.lc/search?q=",
	// Liechtenstein
    "li": "https://www.google.li/search?q=",
	// Sri Lanka
    "lk": "https://www.google.lk/search?q=",
	// Lesotho
    "ls": "https://www.google.co.ls/search?q=",
	// Lithuania
    "lt": "https://www.google.lt/search?q=",
	// Luxembourg
    "lu": "https://www.google.lu/search?q=",
	// Latvia
    "lv": "https://www.google.lv/search?q=",
	// Libya
    "ly": "https://www.google.com.ly/search?q=",
	// Morocco
    "ma": "https://www.google.co.ma/search?q=",
	// Moldova
    "md": "https://www.google.md/search?q=",
	// Montenegro
    "me": "https://www.google.me/search?q=",
	// Madagascar
    "mg": "https://www.google.mg/search?q=",
	// Republic of North Macedonia
    "mk": "https://www.google.mk/search?q=",
	// Mali
    "ml": "https://www.google.ml/search?q=",
	// Myanmar
    "mm": "https://www.google.com.mm/search?q=",
	// Mongolia
    "mn": "https://www.google.mn/search?q=",
	// Montserrat
    "ms": "https://www.google.ms/search?q=",
	// Malta
    "mt": "https://www.google.com.mt/search?q=",
	// Mauritius
    "mu": "https://www.google.mu/search?q=",
	// Maldives
    "mv": "https://www.google.mv/search?q=",
	// Malawi
    "mw": "https://www.google.mw/search?q=",
	// Mexico
    "mx": "https://www.google.com.mx/search?q=",
	// Malaysia
    "my": "https://www.google.com.my/search?q=",
	// Mozambique
    "mz": "https://www.google.co.mz/search?q=",
	// Namibia
    "na": "https://www.google.com.na/search?q=",
	// Niger
    "ne": "https://www.google.ne/search?q=",
	// Norfolk Island
    "nf": "https://www.google.com.nf/search?q=",
	// Nigeria
    "ng": "https://www.google.com.ng/search?q=",
	// Nicaragua
    "ni": "https://www.google.com.ni/search?q=",
	// The Netherlands
    "nl": "https://www.google.nl/search?q=",
	// Norway
    "no": "https://www.google.no/search?q=",
	// Nepal
    "np": "https://www.google.com.np/search?q=",
	// Nauru
    "nr": "https://www.google.nr/search?q=",
	// Niue
    "nu": "https://www.google.nu/search?q=",
	// New Zealand
    "nz": "https://www.google.co.nz/search?q=",
	// Oman
    "om": "https://www.google.com.om/search?q=",
	// Pakistan
    "pk": "https://www.google.com.pk/search?q=",
	// Panama
    "pa": "https://www.google.com.pa/search?q=",
	// Peru
    "pe": "https://www.google.com.pe/search?q=",
	// The Philippines
    "ph": "https://www.google.com.ph/search?q=",
	// Poland
    "pl": "https://www.google.pl/search?q=",
	// Papua New Guinea
    "pg": "https://www.google.com.pg/search?q=",
	// Pitcairn
    "pn": "https://www.google.pn/search?q=",
	// Puerto Rico
    "pr": "https://www.google.com.pr/search?q=",
	// Palestine
    "ps": "https://www.google.ps/search?q=",
	// Portugal
    "pt": "https://www.google.pt/search?q=",
	// Paraguay
    "py": "https://www.google.com.py/search?q=",
	// Qatar
    "qa": "https://www.google.com.qa/search?q=",
	// Romania
    "ro": "https://www.google.ro/search?q=",
	// Serbia
    "rs": "https://www.google.rs/search?q=",
	// Russia
    "ru": "https://www.google.ru/search?q=",
	// Rwanda
    "rw": "https://www.google.rw/search?q=",
	// Saudi Arabia
    "sa": "https://www.google.com.sa/search?q=",
	// Solomon Islands
    "sb": "https://www.google.com.sb/search?q=",
	// Seychelles
    "sc": "https://www.google.sc/search?q=",
	// Sweden
    "se": "https://www.google.se/search?q=",
	// Singapore
    "sg": "https://www.google.com.sg/search?q=",
	// Saint Helena, Ascension and Tristan da Cunha
    "sh": "https://www.google.sh/search?q=",
	// Slovenia
    "si": "https://www.google.si/search?q=",
	// Slovakia
    "sk": "https://www.google.sk/search?q=",
	// Sierra Leone
    "sl": "https://www.google.com.sl/search?q=",
	// Senegal
    "sn": "https://www.google.sn/search?q=",
	// San Marino
    "sm": "https://www.google.sm/search?q=",
	// Somalia
    "so": "https://www.google.so/search?q=",
	// Sao Tome and Principe
    "st": "https://www.google.st/search?q=",
	// Suriname
    "sr": "https://www.google.sr/search?q=",
	// El Salvador
    "sv": "https://www.google.com.sv/search?q=",
	// Chad
    "td": "https://www.google.td/search?q=",
	// Togo
    "tg": "https://www.google.tg/search?q=",
	// Thailand
    "th": "https://www.google.co.th/search?q=",
	// Tajikistan
    "tj": "https://www.google.com.tj/search?q=",
	// Tokelau
    "tk": "https://www.google.tk/search?q=",
	// Timor-Leste
    "tl": "https://www.google.tl/search?q=",
	// Turkmenistan
    "tm": "https://www.google.tm/search?q=",
	// Tonga
    "to": "https://www.google.to/search?q=",
	// Tunisia
    "tn": "https://www.google.tn/search?q=",
	// Turkey
    "tr": "https://www.google.com.tr/search?q=",
	// Trinidad and Tobago
    "tt": "https://www.google.tt/search?q=",
	// Taiwan
    "tw": "https://www.google.com.tw/search?q=",
	// Tanzania, United Republic of
    "tz": "https://www.google.co.tz/search?q=",
	// Ukraine
    "ua": "https://www.google.com.ua/search?q=",
	// Uganda
    "ug": "https://www.google.co.ug/search?q=",
	// United Kingdom
    "uk": "https://www.google.co.uk/search?q=",
	// United States of America
    "us": "https://www.google.com/search?q=",
	// Uruguay
    "uy": "https://www.google.com.uy/search?q=",
	// Uzbekistan
    "uz": "https://www.google.co.uz/search?q=",
	// Saint Vincent and the Grenadines	
    "vc": "https://www.google.com.vc/search?q=",
	// Venezuela
    "ve": "https://www.google.co.ve/search?q=",
	// Virgin Islands (British)	
    "vg": "https://www.google.vg/search?q=",
	// Virgin Islands (U.S.)
    "vi": "https://www.google.co.vi/search?q=",
	// Vietnam	
    "vn": "https://www.google.com.vn/search?q=",
	// Vanuatu
    "vu": "https://www.google.vu/search?q=",
	// Samoa
    "ws": "https://www.google.ws/search?q=",
	// South Africa	
    "za": "https://www.google.co.za/search?q=",
	// Zambia
    "zm": "https://www.google.co.zm/search?q=",
	// Zimbabwe
    "zw": "https://www.google.co.zw/search?q=",
}
```
