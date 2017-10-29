<style scoped>
img {
    width: 300px;
}

body {
    font: 13px arial;
    color: #111;
}

.header a,
.header a:visited,
.header a:active {
    color: #ccf;
}

.content a,
.content a:visited,
.content a:active {
    color: #77b;
}

thead.D {
    background: #eee;
}

thead.F {
    background: #edd;
}

thead.M {
    background: #dde;
}

thead.P {
    background: #ded;
}

thead {
    font-size: 0.8em;
    color: #777;
}

.toolbox {
    width: 750px;
    margin: 0px;
    padding-left: 8px;
    padding-top: 8px;
    padding-bottom: 10px;
    padding-right: 8px;
    border: 1px dotted #777;
}
</style>

<template>
<div class="container">
    <div>
        <div style="margin-left:auto;margin-right:auto;background:#fff;">
            <div style="margin-left:auto;margin-right:auto;padding-top:8px;padding-bottom:64px;" class="header">
                <div style="margin-bottom:16px;margin-left:auto;margin-right:auto;padding-top:4px;padding-bottom:4px;padding-left:4px;background:#333;color:eee;">experimental disc golf flight path visualizer
                    <div style="float:right;margin-right:4px;"><a href="http://www.reddit.com/user/tulihauki/">Send feedback</a></div>
                </div>
                <div style="margin-bottom: 16px;" class="content">
                    This is an HTML5 experiment for visualising the flight path of golf discs at different wear and throwing power levels. It can also be used to identify where the discs in your bag can potentially overlap. Feel free to play around with it - all feedback
                    is welcome.
                    <br/>
                    <br/>Disc flight data is by <a target="_blank" href="http://www.inboundsdiscgolf.com/">inbounds Disc Golf</a>.
                    <br/>
                    <br/>
                </div>
                <div>
                    <div class="col-lg-6 col-md-6 col-sm-8" style="float:left;">
                        <canvas style="border: 1px solid #000" id="splinecanvas" width="500" height="750"></canvas>
                        <discControl :power='power' :throwType='throwType' @changeDiscPower='power = $event' @changeThrowType='throwType = $event'>
                        </discControl>
                        <pathViewControl></pathViewControl>
                    </div>
                    <div class="col-lg-6 col-md-6 col-sm-4">
                        <bag :discs='discs'>Discs In Bag</bag>
                    </div>
                </div>
                <div style="">
                    <span style="font-size:1.3em;font-weight:bold;">Discs in bag</span>
                    <br/>
                    <div style="border-top: 1px solid #000;border-bottom: 1px solid #000">
                        <table style="width:100%" id="bag">
                            <thead class="D">
                              <tr>
                                <th colspan=4>Distance drivers</th>
                              </tr>
                            </thead>
                            <tbody class="D"></tbody>
                            <thead class="F">
                              <tr>
                                <th colspan=4>Fairway drivers</th>
                              </tr>
                            </thead>
                            <tbody class="F"></tbody>
                            <thead class="M">
                              <tr>
                                <th colspan=4>Midranges</th>
                              </tr>
                            </thead>
                            <tbody class="M"></tbody>
                            <thead class="P">
                              <tr>
                                <th colspan=4>Putt and approach</th>
                              </tr>
                            </thead>
                            <tbody class="P"></tbody>
                        </table>
                    </div>
                    <br/>
                    <div>
                      <button @click="addDiscToBag();" style="float:right;">Add to bag</button>
                      <select style="width:76%;float:left;" id="disc" v-model="selectedDisc">
                          <option v-for='(disc, discIndex) in filteredDiscs' :value="disc.data">{{disc.disc}}</option>
                      </select>
                      <div style="clear:both;"></div>
                      <div style="width:100%;float:left;margin-top:3px;">Filter:
                          <input id="discfilter" slze="20" v-model='discFilter' placeholder="Filter list">
                      </div>
                      <div style="clear:both;"></div>
                    </div>
                    <div style="clear:left;"></div>
                </div>
                <div style="clear:both;"></div>
                <br/>
                <b>Throw:</b>
                <br/>
                <div class="settings toolbox">
                  <b>Type:</b>
                  <select id="throw-type">
                    <option value="rhbh" selected>RHBH/LHFH</option>
                    <option value="rhfh">RHFH/LHBH</option>
                  </select> &nbsp;
                  <b>Power:</b>
                  <input type="range" min="0" max="48" value="32" step="1" id="power">
                   (<b><span id="pwrval"></span>%</b> ofb nominal airspeed required by disc)
                </div>
                <br/>
                <b>Display options:</b>
                <br/>
                <div class="settings toolbox">
                  <b>Spread:</b>
                  <input type="checkbox" checked id="fan-power"> &nbsp;
                  <b>Paths per disc:</b>
                  <select id="paths-shown">
                      <option value="all">All</option>
                      <option value="one" selected>One</option>
                      <option value="none">None</option>
                  </select> &nbsp; &nbsp; <b>Labels:</b>
                  <input type="checkbox" id="lie-distance" checked> &nbsp; <b>Circles (10m/15m):</b>
                  <input type="checkbox" id="lie-circle" checked>
                  <!-- &nbsp; <b>Debug:</b> <input type="checkbox" id="debug-data"> -->
                </div>
                <br/>
                <b>Import/export bag:</b>
                <br/>
                <div class="settings toolbox">
                  <button @click="exportBagJSON(0);">Export bag as JSON</button> &nbsp;
                  <button @click="exportBagJSON(1);">Export compressed bag</button> &nbsp; | &nbsp;
                  <button @click="importBagJSON(0);">Import bag from JSON</button> &nbsp;
                  <button @click="importBagJSON(1);">Import compressed bag</button>
                  <br/>
                  <br/>
                  <textarea style="width:100%;margin-right:8px;" id="bag-raw-data" rows="10"></textarea>
                  <br/>
                  <span id="debug-area"></span>
                </div>
            </div>
        </div>
    </div>
</div>
</template>

<script>
import Bag from './Bag.vue';
import PathViewControl from './PathViewControl.vue';
import DiscControl from './DiscControl.vue';
export default {
    data() {
        return {
            "imageURL": "http://ind02.inboundsdiscgolf.com/2380596.png",
            "discs": [],
            "discdata": [],
            "selectedDisc": '',
            "discFilter": '',
            "throwType": 'rhbh',
            "power": 34,
            "fanpower": '',
            "pathsshown": '',
            "liedistance": '',
            "liecircle": ''
        }
    },
    methods: {
        drawPath() {

        },
        addDiscToBag() {
            var d = $("#disc").val().split(';');
            var dn = $("#disc option:selected").html();
            addDisc(d, dn, 10, true);
            updateBag();
        },
        exportBagJSON(method) {
            var bag = updateBag();
            var json;
            if (method) {
                json = JSON.stringify(bag);
                var lzbag = LZString.compressToBase64(json);
                $("#bag-raw-data").val(lzbag);
            } else {
                json = JSON.stringify(bag, null, 2);
                $("#bag-raw-data").val(json);
            }
        },
        importBagJSON(method) {
            var json = "";
            if (method) {
                var lzb = $("#bag-raw-data").val();
                json = LZString.decompressFromBase64(lzb);
            } else {
                json = $("#bag-raw-data").val();
            }
            var ibag = JSON.parse(json);

            // parse and check if data is valid
            if (typeof ibag != 'undefined') {
                $("table#bag tbody").empty();
                for (key in ibag) {
                    if (typeof ibag[key].data != 'undefined' &&
                        typeof ibag[key].disc != 'undefined' &&
                        typeof ibag[key].wear != 'undefined' &&
                        typeof ibag[key].enabled != 'undefined')
                        addDisc(ibag[key].data, ibag[key].disc, ibag[key].wear, ibag[key].enabled);
                }
                updateBag();
            }
        },
        removeDisc(btn) {
            var tr = $(btn).parent().parent();
            $(tr).remove();
            updateBag();
        },
        containsFilter(value) {
            return value.disc.toLowerCase().indexOf(this.discFilter.toLowerCase()) > -1;
        }
    },
    computed: {
        filteredDiscs() {
            return this.discdata.filter(this.containsFilter)
        }
    },
    mounted() {
        var self = this;
        if (localStorage["bag"]) {
            this.discs = JSON.parse(localStorage["bag"]);
        }
        $.getJSON("./discdata.json", function(json) {
            self.discdata = json;
        });

        if (localStorage["throw-type"]) this.throwType = localStorage["throw-type"];
        if (localStorage["power"]) this.power = localStorage["power"];
        if (localStorage["fan-power"]) this.fanpower = localStorage["fan-power"];
        if (localStorage["paths-shown"]) this.pathsshown = localStorage["paths-shown"];
        if (localStorage["lie-distance"]) this.liedistance = localStorage["lie-distance"];
        if (localStorage["lie-circle"]) this.liecircle = localStorage["lie-circle"];

    },
    components: {
        'bag': Bag,
        'pathViewControl': PathViewControl,
        'discControl': DiscControl
    }
}

var yscale = 2.5,
    xscale = 0.7;
var canvas;
var pathBuffer, lieBuffer, outlineBuffer;

// rgb spline points
var spr = [0, 0, 0, 0, 0, 0, 255, 255];
var spg = [255, 255, 255, 255, 255, 255, 255, 0];
var spb = [0, 0, 0, 0, 0, 0, 0, 0];

// return formatted 8-bit hex byte for integer n
function hb(n) {
    var s = Math.floor(n).toString(16);
    if (s.length == 1) s = '0' + s;
    return s;
}

// catmull-rom spline interpolation
function catmull(p, i, pc) {
    var y0, y1, y2, y3;
    var a, b, c, d;
    var k = Math.floor(i * (pc - 1));
    var t = i * (pc - 1) - k;
    y0 = (k > 0) ? p[k - 1] : p[0] + (p[0] - p[1]);
    y1 = p[k];
    y2 = (k < (pc - 1)) ? p[k + 1] : p[pc - 1];
    y3 = (k < (pc - 2)) ? p[k + 2] : p[pc - 1] + (p[pc - 1] - p[pc - 2]);
    a = y1 * 2.0;
    b = -y0 + y2;
    c = y0 * 2.0 - y1 * 5.0 + y2 * 4.0 - y3;
    d = -y0 + y1 * 3.0 - y2 * 3.0 + y3;
    return 0.5 * (a + b * t + c * t * t + d * t * t * t);
}

// clear canvas and render buffers
function resetBuffers() {
    var context = canvas.getContext("2d");
    context.fillStyle = "#000";
    context.fillRect(0, 0, canvas.width, canvas.height);
    context.lineWidth = 1.0;
    context.font = "9px helvetica";
    context.fillStyle = "#999";
    context.strokeStyle = "#446";
    var i;
    for (i = 0; i < canvas.width; i += 50) {
        context.beginPath();
        context.moveTo(i, 0);
        context.lineTo(i, canvas.height);
        context.stroke();
    }
    for (i = 0; i <= canvas.height; i += 50) {
        context.beginPath();
        context.moveTo(0, i);
        context.lineTo(canvas.height, i);
        context.stroke();
        context.textAlign = "left";
        context.fillText((canvas.height - i) + "'", 0, i - 3);
        context.textAlign = "right";
        context.fillText(Math.floor((canvas.height - i) / 3.33) + "m", canvas.width, i - 3);
    }
    var pathContext = pathBuffer.getContext("2d");
    var lieContext = lieBuffer.getContext("2d");
    var outlineContext = outlineBuffer.getContext("2d");
    pathContext.clearRect(0, 0, canvas.width, canvas.height);
    lieContext.clearRect(0, 0, canvas.width, canvas.height);
    outlineContext.clearRect(0, 0, canvas.width, canvas.height);
}

// draw a lie to disc landing coordinates
function drawLie(x, y, markLie, color, lieColor, lieOutline) {
    var pathContext = pathBuffer.getContext("2d");
    var lieContext = lieBuffer.getContext("2d");
    var outlineContext = outlineBuffer.getContext("2d");

    // mark the lie
    if (markLie) {
        pathContext.strokeStyle = color;
        pathContext.fillStyle = color;
        pathContext.beginPath();
        pathContext.arc(x, y, 2, 0, 2 * 3.1415926);
        pathContext.stroke();
        pathContext.fill();
    }

    // 15m circle around lie
    outlineContext.globalCompositeOperation = "source-over";
    outlineContext.globalAlpha = 1.0;
    outlineContext.fillStyle = lieOutline;
    outlineContext.strokeStyle = lieOutline;
    outlineContext.beginPath();
    outlineContext.arc(x, y, 1.5 * 33, 0, 2 * 3.1415926);
    outlineContext.stroke();
    outlineContext.fill();

    // 10m circle around lie
    lieContext.globalCompositeOperation = "source-over";
    lieContext.globalAlpha = 1.0;
    lieContext.fillStyle = lieColor;
    lieContext.strokeStyle = lieColor;
    lieContext.beginPath();
    lieContext.arc(x, y, 33, 0, 2 * 3.1415926);
    lieContext.stroke();
    lieContext.fill();
}

// draw a single disc trajectory path
function drawPath(dist, hss, lsf, armspeed, wear, color, drawPath) {
    var pathContext = pathBuffer.getContext("2d");
    pathContext.strokeStyle = color;
    pathContext.lineWidth = 2.4;
    var airspeed = armspeed;
    var ehss = hss,
        elsf = lsf;
    var turnsign = ($("#throw-type").val() == "rhbh") ? 1.0 : -1.0;
    var fadestart = 0.4 + (1.0 - airspeed * airspeed) * 0.3;
    var impact = (1.0 - airspeed) / 5;
    var turnend = 0.8 - airspeed * airspeed * 0.36;
    var x, y;
    var ox = canvas.width / 2;
    var oy = canvas.height;
    var vx = 0.0,
        vy = -1.0;
    var ht = yscale * dist;
    var deltav = yscale / ht;
    var wm = wear / 10.0;
    // calculate effective HSS and LSF
    ehss *= 1.0 + 1.0 - wm;
    ehss -= ((1.0 - wm) / 0.05) * (dist / 100);
    elsf *= wm;
    if (airspeed > 0.8) {
        var op = (airspeed - 0.8) / 0.4;
        op *= op * 2;
        var dc = Math.max(0.0, (350 - dist)) / 10.0; // emphasize high-speed turn on sub-350ft discs
        ehss -= op * dc;
    }
    ehss *= airspeed * airspeed * airspeed * airspeed;
    elsf *= 1.0 / (airspeed * airspeed);

    // iterate through the flight path
    var yd, yt, yf;
    do {
        y = oy + vy;
        x = ox + vx * xscale;
        airspeed -= deltav;
        if (airspeed > turnend) {
            vx -= turnsign * (ehss / 14000) * (turnend / airspeed);
            yt = canvas.height - y;
        }
        if (airspeed < fadestart) {
            vx -= turnsign * (elsf / 4000) * (fadestart - airspeed) / fadestart;
        } else {
            yf = canvas.height - y;
        }
        if (airspeed > 0.0) {
            if (drawPath) {
                pathContext.beginPath();
                pathContext.moveTo(ox, oy);
                pathContext.lineTo(x, y);
                pathContext.stroke();
            }
            ox = x;
            oy = y;
        }
    } while (airspeed > impact);
    yd = canvas.height - oy;
    if ($("#debug-data").is(":checked")) {
        if (drawPath) {
            var hx = canvas.width / 2;
            console.log("hss " + hss + " lsf " + lsf + " ehss " + ehss + " elsf " + elsf + " turnend " + turnend + " fadestart " + fadestart + " yd " + yd);
            pathContext.strokeStyle = "#fff";
            pathContext.beginPath();
            pathContext.moveTo(hx - 20, canvas.height - yd);
            pathContext.lineTo(hx + 20, canvas.height - yd);
            pathContext.stroke();

            pathContext.strokeStyle = "#0ff";
            pathContext.beginPath();
            pathContext.moveTo(hx - 20, canvas.height - yt);
            pathContext.lineTo(hx + 20, canvas.height - yt);
            pathContext.stroke();

            pathContext.strokeStyle = "#f0f";
            pathContext.beginPath();
            pathContext.moveTo(hx - 20, canvas.height - yf);
            pathContext.lineTo(hx + 20, canvas.height - yf);
            pathContext.stroke();
        }
    }
    // return lie coordinates to caller
    return [x, y];
}

// update canvas from inputs on page
function updateCanvas() {
    var pwi = parseInt($("#power").val());
    var pw = 0.6 + (pwi / 48) * 0.6;
    $("#pwrval").html(Math.floor(pw * 100));
    var pc;
    var pws;
    var drawPaths = $("#paths-shown").val();
    var lieColors = {
        D: '#fff',
        F: '#faa',
        M: '#aaf',
        P: '#afa'
    };
    var lieOutlines = {
        D: '#888',
        F: '#833',
        M: '#338',
        P: '#383'
    };
    var lie;

    resetBuffers();
    var lieLabels = [];
    $("td.disc-in-bag").each(function() {
        if (!$("input,disc-enabled", this).is(":checked")) return;
        var d = $("input.disc-data", this).val().split(",");
        var dn = $("input.disc-name", this).val();
        var dt = d[3];
        var dw = $("select.disc-wear", $(this).parent()).val();
        var ry;
        // draw fan/landing zone if power spread enabled
        if ($("#fan-power").is(":checked")) {
            for (var i = 0; i <= 24; i++) {
                var pws = (i / 24.0);
                ry = Math.min(255, Math.max(0, Math.floor(catmull(spr, pws, 8))));
                var gy = Math.min(255, Math.max(0, Math.floor(catmull(spg, pws, 8))));
                var by = Math.min(255, Math.max(0, Math.floor(catmull(spb, pws, 8))));
                var pwf = 0.6 + pws * 0.6;
                var deltap = Math.abs(pw - pwf);
                var a = Math.min(0.4, Math.max(0.3, Math.cos(deltap * 5.5)));
                pc = "#" + hb(a * ry) + hb(a * gy) + hb(a * by);
                lie = drawPath(parseInt(d[0]), parseInt(d[1]), parseInt(d[2]), pwf, dw, pc, (drawPaths == "all") && (i % 2 == 0));
                drawLie(lie[0], lie[1], (drawPaths == "all") && (i % 2 == 0), pc, lieColors[dt], lieOutlines[dt]);
            }
        }

        // draw disc path for selected throw power
        pws = (pwi / 48.0);
        ry = Math.min(255, Math.max(0, Math.floor(catmull(spr, pws, 8))));
        gy = Math.min(255, Math.max(0, Math.floor(catmull(spg, pws, 8))));
        by = Math.min(255, Math.max(0, Math.floor(catmull(spb, pws, 8))));
        pc = "#" + hb(ry) + hb(gy) + hb(by);
        lie = drawPath(parseInt(d[0]), parseInt(d[1]), parseInt(d[2]), pw, dw, pc, (drawPaths == "all" || drawPaths == "one"));
        drawLie(lie[0], lie[1], true, pc, lieColors[dt], lieOutlines[dt]);
        lieLabels.push([lie, dn]);
    });

    // overlay on base image
    var context = canvas.getContext("2d");
    var pathContext = pathBuffer.getContext("2d");
    var lieContext = lieBuffer.getContext("2d");
    var outlineContext = outlineBuffer.getContext("2d");
    if ($("#lie-circle").is(":checked")) {
        context.globalAlpha = 0.35;
        context.globalCompositeOperation = "source-over";
        context.drawImage(outlineBuffer, 0, 0);
        context.globalAlpha = 0.15;
        context.globalCompositeOperation = "source-over";
        context.drawImage(lieBuffer, 0, 0);
        context.globalAlpha = 1.0;
        context.globalCompositeOperation = "source-over";
    }
    context.drawImage(pathBuffer, 0, 0);

    // write lie distance if enabled
    if ($("#lie-distance").is(":checked")) {
        for (var key in lieLabels) {
            lie = lieLabels[key][0];
            var dn = lieLabels[key][1];
            var txt = (canvas.height - lie[1]) + "'  " + Math.floor((canvas.height - lie[1]) / 3.33) + "m";
            context.font = "10px helvetica";
            context.textAlign = "center";
            context.strokeStyle = "#000"
            context.fillStyle = "#c0ffee";
            context.lineWidth = 3;
            context.strokeText(txt, lie[0], lie[1] - 6);
            context.fillText(txt, lie[0], lie[1] - 6);
            context.font = "9px helvetica";
            context.strokeText(dn, lie[0], lie[1] - 18);
            context.fillText(dn, lie[0], lie[1] - 18);
        }
    }
}

// update the config in local storage
function updateSettings() {
    if (typeof(Storage) !== "undefined") {
        localStorage["throw-type"] = $("#throw-type").val();
        localStorage["power"] = $("#power").val();
        localStorage["fan-power"] = $("#fan-power").is(":checked");
        localStorage["paths-shown"] = $("#paths-shown").val();
        localStorage["lie-distance"] = $("#lie-distance").is(":checked");
        localStorage["lie-circle"] = $("#lie-circle").is(":checked");
    }
    updateCanvas();
}

var bag = [];
// update the bag in local storage and redraw
function updateBag() {
    var bag = [];
    if (typeof(Storage) !== "undefined") {
        $("td.disc-in-bag").each(function() {
            var e = $("input,disc-enabled", this).is(":checked");
            var d = $("input.disc-data", this).val().split(",");
            var dn = $("input.disc-name", this).val();
            var dw = $("select.disc-wear", $(this).parent()).val();
            bag.push({
                disc: dn,
                data: d,
                wear: dw,
                enabled: e
            });
        });
        var j = JSON.stringify(bag);
        localStorage["bag"] = JSON.stringify(bag);
    }
    updateCanvas();
    return bag;
}

// append a disc row to the table
function addDisc(d, dn, dw, e) {
    var dt = d[3];
    var el = '<tr><td class="disc-in-bag">' +
        '<input class="disc-data" type="hidden" value="' + d + '">' +
        '<input class="disc-name" type="hidden" value="' + dn + '">' +
        '<input class="disc-enabled" type="checkbox" ' + (e ? "checked" : "") + ' onchange="updateBag();"></td><td width="100%" class="disc-name">' + dn + '</td>' +
        '<td><select class="disc-wear" onchange="updateBag();">';
    for (var i = 10; i > 0; i--) el += '<option value="' + i + '"' + ((dw == i) ? ' selected' : '') + '>' + i + '/10</option>';
    el += '</td>';
    el += '<td><button @click="removeDisc(this);">Remove</button></td></tr>'
    $("#bag tbody." + dt).append(el);
}

function updateSelectBox() {
    var f = $("input#discfilter").val();
    var re = new RegExp(f, "i");
    $("select#disc option").remove();
    for (var k in window.discdata) {
        var d = window.discdata[k];
        if (re.test(d['disc']))
            $("select#disc").append('<option value="' + d['data'] + '">' + d['disc'] + '</option>');
    }
}

// setup the application when DOM is ready
$(document).ready(function() {
    // get canvas and create off-screen buffers
    canvas = document.getElementById("splinecanvas");
    pathBuffer = document.createElement('canvas');
    pathBuffer.width = canvas.width;
    pathBuffer.height = canvas.height;
    lieBuffer = document.createElement('canvas');
    lieBuffer.width = canvas.width;
    lieBuffer.height = canvas.height;
    outlineBuffer = document.createElement('canvas');
    outlineBuffer.width = canvas.width;
    outlineBuffer.height = canvas.height;

    // read bag and settings from local storage
    if (typeof(Storage) !== "undefined") {
        if (localStorage["bag"]) {
            var bag = JSON.parse(localStorage["bag"]);
            for (var key in bag) addDisc(bag[key].data, bag[key].disc, bag[key].wear, bag[key].enabled);
        }
    }
    if (typeof(Storage) !== "undefined") {
        if (localStorage["throw-type"]) $("#throw-type").val(localStorage["throw-type"]);
        if (localStorage["power"]) $("#power").val(localStorage["power"]);
        if (localStorage["fan-power"]) $("#fan-power").attr("checked", localStorage["fan-power"] == "true");
        if (localStorage["paths-shown"]) $("#paths-shown").val(localStorage["paths-shown"]);
        if (localStorage["lie-distance"]) $("#lie-distance").attr("checked", localStorage["lie-distance"] == "true");
        if (localStorage["lie-circle"]) $("#lie-circle").attr("checked", localStorage["lie-circle"] == "true");
    }

    // register event handlers for inputs
    $("div.settings input").on("input", updateSettings);
    $("div.settings input").on("change", updateSettings);
    $("div.settings select").on("change", updateSettings);
    //var jsontest = require('./discdata.json');

    $.getJSON("./discdata.json", function(json) {
        window.discdata = json;
        updateSelectBox();
        console.log(window.discdata); // this will show the info it in firebug console
    });
    // store full list of discs
    // populate select box and select TD Rush :D
    $("input#discfilter").val("");
    updateSelectBox();
    $("select#disc option:contains('Discmania TD Rush')").attr("selected", "selected");
    $("input#discfilter").on("input", updateSelectBox);

    // paint the canvas for the first time
    updateCanvas();
});
</script>
