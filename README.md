example-fin-hypergrid-behavior-json
===================================

see the [json hypergrid here](http://openfin.github.io/example-fin-hypergrid-behavior-json/)

example usage of the polymer based hypergrid json behavior


git clone https://github.com/openfin/example-fin-hypergrid-behavior-json.git

bower install

run your favorite web server in the created directory

<pre>


&lt;!doctype html>
&lt;html>
&lt;head>
  &lt;meta name="viewport" content="width=device-width, minimum-scale=1.0, initial-scale=1.0, user-scalable=yes">
  &lt;title>fin-hypergrid Demo&lt;/title>
  &lt;script src="bower_components/webcomponentsjs/webcomponents.js">&lt;/script>
  &lt;link rel="import" href="fin-hypergrid.min.html">
  &lt;style>
    fin-hypergrid {
        position: absolute;
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
    }
  &lt;/style>
&lt;/head>
&lt;body unresolved>

    &lt;fin-hypergrid id="json-example">
        &lt;fin-hypergrid-behavior-json>&lt;/fin-hypergrid-behavior-json>
    &lt;/fin-hypergrid>

    &lt;script>
        //setup random data for the JSON tab example
        (function(){
            var numRows = 5000;
            var firstNames = ['Olivia', 'Sophia', 'Ava', 'Isabella', 'Boy', 'Liam', 'Noah', 'Ethan', 'Mason', 'Logan', 'Moe', 'Larry', 'Curly', 'Shemp', 'Groucho', 'Harpo', 'Chico', 'Zeppo', 'Stanley', 'Hardy'];
            var lastNames = ['Wirts', 'Oneil', 'Smith', 'Barbarosa', 'Soprano', 'Gotti', 'Columbo', 'Luciano', 'Doerre', 'DePena'];
            var months = ['01', '02', '03', '04', '05', '06', '07', '08', '09', '10', '11', '12'];
            var days = ['01', '02', '03', '04', '05', '06', '07', '08', '09', '10', '11', '12', '13', '14', '15', '16', '17', '18', '19', '20', '21', '22', '23', '24', '25', '26', '27', '28', '29', '30'];
            var states = ['Alabama','Alaska','Arizona','Arkansas','California','Colorado','Connecticut','Delaware','Florida','Georgia','Hawaii','Idaho','Illinois','Indiana','Iowa','Kansas','Kentucky','Louisiana','Maine','Maryland','Massachusetts','Michigan','Minnesota','Mississippi','Missouri','Montana','Nebraska','Nevada','New Hampshire','New Jersey','New Mexico','New York','North Carolina','North Dakota','Ohio','Oklahoma','Oregon','Pennsylvania','Rhode Island','South Carolina','South Dakota','Tennessee','Texas','Utah','Vermont','Virginia','Washington','West Virginia','Wisconsin','Wyoming'];
            var randomPerson = function() {
                var firstName = Math.round((firstNames.length - 1) * Math.random());
                var lastName = Math.round((lastNames.length - 1) * Math.random());
                var pets = Math.round(10 * Math.random());
                var birthyear = 1900 + Math.round(Math.random() * 114);
                var birthmonth = Math.round(Math.random() * 11);
                var birthday = Math.round(Math.random() * 29);
                var birthstate = Math.round(Math.random() * 49);
                var residencestate = Math.round(Math.random() * 49);
                var score = Math.round(Math.random() * 1000000)/1000;
                var income = Math.round(Math.random() * 10000000)/100;
                var employed = Math.round(Math.random());
                var person = {
                    last: lastNames[lastName],
                    first: firstNames[firstName],
                    pets: pets,
                    birthdate: birthyear + '-' + months[birthmonth] + '-' + days[birthday],
                    birthstate: states[birthstate],
                    residencestate: states[residencestate],
                    employed: employed === 1,
                    income: income,
                    score: score
                };
                return person;
            };
           var headers =
                [
                    'last',
                    'first',
                    'pets',
                    'birth date',
                    'birth state',
                    'residence state',
                    'employed',
                    'income',
                    'score'
                ];
            var fields =
                [
                    'last',
                    'first',
                    'pets',
                    'birthdate',
                    'birthstate',
                    'residencestate',
                    'employed',
                    'income',
                    'score'
                ];
            var data = [];
            for (var i = 0; i &lt; numRows; i ++) {
                data.push(randomPerson());
            }
            var editorTypes = ['choice','textfield','spinner','date','choice','choice','choice','textfield','textfield'];
            document.addEventListener('polymer-ready', function() {
                //get ahold of our json grid example
                var jsonGrid = document.querySelector('#json-example');
                //get it's table model
                var jsonModel = jsonGrid.getBehavior();
                //get the cell cellProvider for altering cell renderers
                var cellProvider = jsonModel.getCellProvider();
                //set the header labels
                jsonModel.setHeaders(headers);
                //set the fields found on the row objects
                jsonModel.setFields(fields);
                //set the actual json row objects
                jsonModel.setData(data);
                //sort ascending on the first column (first name)
                jsonModel.toggleSort(0);
                //all formatting and rendering per cell can be overridden in here
                cellProvider.getCell = function(config) {
                    var renderer = cellProvider.cellCache.simpleCellRenderer;
                    config.halign = 'left';
                    var x = config.x;
                    if (x === 2) {
                        config.halign = 'center';
                    } else if (x === 3) {
                        config.halign = 'center';
                    } else if (x === 6) {
                        config.halign = 'center';
                    } else if (x === 7) {
                        var score = 60 + Math.round(config.value*150/100000);
                        var bcolor = score.toString(16);
                        config.halign = 'right';
                        config.bgColor = '#00' + bcolor + '00';
                        config.fgColor = '#FFFFFF';
                    } else if (x === 8) {
                        var score = 105 + Math.round(config.value*150/1000);
                        var bcolor = score.toString(16);
                        config.halign = 'right';
                        config.bgColor = '#' + bcolor+ '0000';
                        config.fgColor = '#FFFFFF';
                    }
                    renderer.config = config;
                    return renderer;
                };
                //lets override the cell editors, and configure the drop down lists
                jsonModel.getCellEditorAt = function(x, y) {
                    var type = editorTypes[x];
                    var cellEditor = this.grid.cellEditors[type];
                    if (x === 0) {
                        cellEditor.items = lastNames;
                    } else if (x === 4 || x === 5) {
                        cellEditor.items = states;
                    } else if (x === 6) {
                        cellEditor.items = [true, false];
                    }
                    return cellEditor;
                };
            });
        })();
    &lt;/script>

&lt;/body>
&lt;/html>

</pre>
