# A simple text file based LCD controller script

My initial idea was to build a full blown web application to drive the [LCD Matrix Display](https://github.com/halfbyte/esp32_matrix_display) I'm building but then I thought, why not just
running a simple script over a couple of files. It should be fairly easy to add a web frontend for this later (multi user support is not an issue, so I'm not worrying about parallel access).

The script will make sure the length restriction of the display (currently 2k for memory management reasons) are not exceeded.

## Dependencies

This script is written in ruby and needs Ruby 2.5+, as it uses the Liquid templating engine.

## Setup

Create a data directory somewhere that has two folders: `texts` and `variables`.

Run `bundle install` to install the ruby dependencies.

## Usage

Create txt files under data/texts to contain the texts you want to display. They can contain Liquid variables. Variables can also be text files (but need to be called .var.txt and live under data/variables.

The text files are sorted before they are concatenated, which means that you can name them with a numeric prefix and they will be displayed in that order.

Run `bin/update $path_to_data_dir` and the output is a string that is less than 2k long. Texts that exceed that length will be discarded.

That output can be used to update the display via curl, for example, like this:

```
$ curl -i -d"Message=`bin/update data`" http://lcddisplay.local/update
```

## Why Variables

The idea is that you could set variables independently of the texts, so you could have a script that updates a variable from, say, a web API. Currently variables are simple values, but I am thinking about allowing the files to be JSON files, so you could curl a JSON file from somewhere and save it as a variable file, but I haven't put enough thought about how to access the JSON structure from the liquid tags (that may actually work automatically?)

## Why not including the http call to upload the text

Small things loosely joined, etc. etc.


## License

This project is licensed under the GNU Affero License version 3. (See [LICENSE.txt](LICENSE.txt))


## A