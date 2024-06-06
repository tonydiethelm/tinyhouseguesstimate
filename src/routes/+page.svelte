<style>
/*
Mobile First! But there's no needed navigation here, this is SPA. 
Frankly, this isn't a mobile kind of thing, they'll need to open tabs for pricing. 
Still, run it down the middle, separate sections with pictures or horizontal rules? 
We want a running cost DIV floating on the upper right corner. 



*/

#floatingCost{
    position: fixed;
    top: 0px;
    right: 1%;
    z-index: 99;
    border-radius: 10px;
    border-style: solid;
    border-color: black;
    padding: 10px;
    background-color: white;
}

div {
    padding: 1% 5%;
}


</style>

<script>


//basic information
$: totalCost = trailerCost + costOfStuds + costOfFloorJoists + costOfRoofJoists + sheathingPlyCost
+ houseWrapCost + roofingUnderlaymentCost + roofingCost + doorUnitCost + windowsCost + sidingCost
+ drywallCost + flooringCost + electricalCost + plumbingCost + cabinetsCost;
let length = 20;
let width = 8;

//trailer and framing costs
//assume 2x6 cost is just 6/4 of 2x4 cost
let trailerCost = 5480;
let twoByFourUnitCost = 3.73;
$: twoByFourBFCost = twoByFourUnitCost/8;
$: twoBySixBFCost = twoByFourBFCost*6/4;
$: numberOfStuds = 2*(length+width)*(12/16)+4;
$: costOfStuds = numberOfStuds * twoByFourUnitCost;
$: numberOfFloorJoists = length*(12/16)+1;
$: numberOfRoofJoists = numberOfFloorJoists + 2;
$: costOfFloorJoists = numberOfFloorJoists * width * twoBySixBFCost;
$: costOfRoofJoists = numberOfRoofJoists * width * twoBySixBFCost;

//sheathing
let sheathingPlyUnitCost = 25;
$: circumference = length*2+width*2;
$: numberOfWallSheathingPly = circumference / 4;
$: numberOfRoofAndFloorSheathingPly = 2*(length /4);
$: sheathingPlyCost = sheathingPlyUnitCost * (numberOfWallSheathingPly+numberOfRoofAndFloorSheathingPly);

//housewrap
//assume housewrap is 10' wide, just need to cover circumference.
let houseWrapUnitCost = 225;
let houseWrapCost;
if(circumference < 150){
    houseWrapCost = houseWrapUnitCost;
}
if(150 < circumference < 300){
    houseWrapCost = houseWrapUnitCost*2;
}

//roofing
//assume roofing panel is 10'x3', or 30sqft
let graceIceWaterShieldUnitCost = 246;
let roofingUnderlaymentCost;
if(length*width < 225){
    roofingUnderlaymentCost = graceIceWaterShieldUnitCost;
}
if(225 < length*width < 450){
    roofingUnderlaymentCost = graceIceWaterShieldUnitCost*2;
}
let roofingUnitCost = 92;
let roofingCost = length * width / 30 * roofingUnitCost;


//windows + door
//assume one door
let numberOfWindows = 6;
let windowUnitCost = 300;
$: windowsCost = numberOfWindows * windowUnitCost;
let doorUnitCost = 365;


//electrical
//assume circumference x2 wiring length and an outlet every 4?
let panelUnitCost = 32;
let wire12gUnitCost = 140;
let electricalOutletUnitCost = 4;
let electricalBoxUnitCost = 1;
$: electricalCost = panelUnitCost + wire12gUnitCost + (circumference/4)*(electricalOutletUnitCost +
    electricalBoxUnitCost
);

//plumbing
//sink
//shower
//water heater
let sinkUnitCost = 200;
let showerUnitCost = 1130;
let waterheaterUnitCost = 670;
$: plumbingCost = sinkUnitCost + showerUnitCost + waterheaterUnitCost;


//drywall
let drywallUnitCost = 18;
$: drywallCost = circumference/4*drywallUnitCost;

//flooring
let flooringUnitCost = 1;
$: flooringCost = (length*width)*flooringUnitCost;

//cabinets
//assume galley kitchen, 2 sets of cabinets. 
let cabinetUnitCost = 320;
$: cabinetsCost = cabinetUnitCost*2;

//siding
//assume siding is 4x8 panel
let sidingUnitCost = 45;
$: sidingCost = circumference/4*sidingUnitCost;




</script>


<!--
use pre filled out forms to gather needed data to do cals. 
Do calcs
spit out guesstimate


Intro text
We are assuming standard framing, 16"OC. We are assuming just one floor. 
--
We need some basic info about your Tiny House. 
Trailer cost: 
Length:
width:
Number of windows:
--
We need some basic info about the cost of things. 
Cost of a 2x4:
Cost of a piece of plywood for sheathing:
Cost of water barrier:
Cost of insulation:
Cost of roofing:
Cost of a window:
Cost of siding:
Cost of a door:
Cost of a shower:
Cost of cabinets:
Cost of a sink:
Cost of an electrical panel:
Cost of 12g wire:
Cost of flooring:
Cost of drywall:

-->
<div id='floatingCost'>
<p>Total Cost: ${(totalCost*1.15).toFixed(2)}</p>

</div>


<h1>Welcome to the Tiny House Cost Guesstimator</h1>

<div id='intro'>
    <p>This is meant to be a cost guesstimator for a Tiny House. 
        Please fill in the text boxes below and I'll do napkin math as we go along.</p>
    <p>Note! I'm NOT being precise here! 
        This is napkin math! I am making some fairly gross assumptions here. 
        Still, it'll get you in the right ballpark.</p>
    <p>I am providing links to Home Depot to get costs for stuff. 
        Don't take this as an endorsement of Home Depot. I just picked a big box store. 
        I highly recommend you go to someplace with properly dried lumber. 
        Big Box Store stuff is generally wet and will warp on you during your build, which is 
        really frustrating. </p>
</div>

<hr>

<div id='basicInputs'>
    <h3>Basic Stuff</h3>
    <p>Length:<input bind:value={length} placeholder=20></p>
    <p>Width:<input bind:value={width} placeholder=8></p>
</div>

<hr>

<div id='stuffCostInputs'>
    <h3>Cost Inputs</h3>
    <p>These costs are vaguely accurate as of the middle of 2024.</p>
    <p>Cost of your <a target="_blank" rel="noopener noreferrer" href="https://www.ironeagletinyhousetrailers-mobile.com/pricing-dimensions">trailer</a>:<input bind:value={trailerCost}></p>
    <p>Cost of a <a target="_blank" rel="noopener noreferrer" href="https://www.homedepot.com/p/2-in-x-4-in-x-8-ft-Prime-Stud-058449/312528776">2x4</a>:<input bind:value={twoByFourUnitCost}></p>
    <p>Cost of <a target="_blank" rel="noopener noreferrer" href="https://www.homedepot.com/p/15-32-in-x-4-ft-x-8-ft-Sheathing-Plywood-Actual-0-438-in-x-48-in-x-96-in-20159/206827282">sheathing ply</a>:<input bind:value={sheathingPlyUnitCost}></p>
    <p>Cost of 9' by 150' <a target="_blank" rel="noopener noreferrer" href="https://www.homedepot.com/p/TYVEK-9-ft-x-150-ft-HomeWrap-Housewrap-1350-Sq-Ft-D15540826/308793219">housewrap</a>:<input bind:value={houseWrapUnitCost}></p>
    <p>Cost of <a target="_blank" rel="noopener noreferrer" href="https://www.homedepot.com/p/GCP-Applied-Technologies-Grace-Ice-and-Water-Shield-36-in-x-75-ft-Roll-Self-Adhered-Roofing-Underlayment-225-sq-ft-5003002/202088840">225sqft roll of Grace Ice and Water Shield, or equivalent</a>:<input bind:value={graceIceWaterShieldUnitCost}></p>
    <p>number of windows: <input bind:value={numberOfWindows}></p>
    <p>Cost of a window:<input bind:value={windowUnitCost}></p>
    <p>Cost of <a target="_blank" rel="noopener noreferrer" href="https://www.homedepot.com/b/Building-Materials-Roofing-Roof-Panels-Metal-Roofing/10/N-5yc1vZapwhZ1z0sdg9">10' by 3' metal roofing panel</a>:<input bind:value={roofingUnitCost}></p>
    <p>Cost of a <a  target="_blank" rel="noopener noreferrer" href="https://www.homedepot.com/p/JELD-WEN-32-in-x-80-in-9-Lite-Primed-Steel-Prehung-Left-Hand-Inswing-Entry-Door-with-Brickmould-735641/202036412">door</a>:<input bind:value={doorUnitCost}></p>
    <p>Cost of a <a  target="_blank" rel="noopener noreferrer" href="https://www.homedepot.com/p/Plytanium-Plywood-Siding-Panel-T1-11-8-IN-OC-Nominal-19-32-in-x-4-ft-x-8-ft-Actual-0-563-in-x-48-in-x-96-in-113699/100000016">siding panel</a>:<input bind:value={sidingUnitCost}></p>
    <p>Cost of a <a  target="_blank" rel="noopener noreferrer" href="https://www.homedepot.com/p/USG-Sheetrock-Brand-1-2-in-x-4-ft-x-8-ft-UltraLight-Drywall-14113411708/202530243">drywall panel</a>:<input bind:value={drywallUnitCost}></p>
    <p>Cost of <a  target="_blank" rel="noopener noreferrer" href="https://www.homedepot.com/p/TrafficMaster-Jessamine-Oak-7-mm-T-x-7-5-in-W-Laminate-Wood-Flooring-26-8-sqft-case-TM1/320125355">1sqft of laminate flooring</a>:<input bind:value={flooringUnitCost}></p>
    <p>Cost of a small electrical panel:<input bind:value={panelUnitCost}></p>
    <p>Cost of roll of <a  target="_blank" rel="noopener noreferrer" href="https://www.homedepot.com/p/Southwire-250-ft-12-2-Solid-Romex-SIMpull-CU-NM-B-W-G-Wire-28828269/202019375">12g wire</a>:<input bind:value={wire12gUnitCost}></p>
    <p>Cost of a small kitchen sink:<input bind:value={sinkUnitCost}></p>
    <p>Cost of a <a  target="_blank" rel="noopener noreferrer" href="https://www.homedepot.com/p/Aquatic-Everyday-Smooth-Tile-36-in-x-36-in-x-76-in-1-Piece-Shower-Stall-with-Center-Drain-in-White-1363STC-WHHD/207038649">one piece shower</a>:<input bind:value={showerUnitCost}></p>
    <p>Cost of a <a  target="_blank" rel="noopener noreferrer" href="https://www.homedepot.com/p/Rinnai-Value-Series-Outdoor-5-6-GPM-Residential-120-000-BTU-Propane-Gas-Tankless-Water-Heater-V53DeP/307268986">propane on demand water heater</a>:<input bind:value={waterheaterUnitCost}></p>
    <p>Cost of a <a  target="_blank" rel="noopener noreferrer" href="https://www.ikea.com/us/en/p/knoxhult-base-cabinet-with-doors-and-drawer-white-00372260/">cabinet set</a>:<input bind:value={cabinetUnitCost}></p>

    <!--
    <p>Cost of a <a  target="_blank" rel="noopener noreferrer" href="h">12g wire</a>:<input bind:value={}></p>
    -->
    



</div>

<hr>

<div id='explanations'>
    <h3>Rough Math Explanations/assumptions</h3>
    <p>I'm assuming your walls are 8' tall for easy math. They'll be taller.
        You'll need more studs and more sheathing than I'm calculating for. 
    </p>
    <p>I'm assuming a stud every 16", and I'm not calculating for extra studs around windows/doors.</p>
    <p>I'm assuming a 2x6 costs about 6/4 of a 2x4.</p>

    <p>For {length}' by {width}', assuming 8' tall walls and 16" OC studs, you need about 
    {numberOfStuds} 2x4 studs at about ${costOfStuds}, {numberOfFloorJoists} 2x6 floor joists, and 
    {numberOfRoofJoists} 2x6 roof joists at about ${(costOfFloorJoists+costOfRoofJoists).toFixed(2)}. </p>

    <p>You'll need about {numberOfWallSheathingPly} wall sheathing plywood pieces and about 
        {numberOfRoofAndFloorSheathingPly} number of roof and floor sheathing plywood pieces, 
    which will be about ${sheathingPlyCost.toFixed(2)}. Honestly, you should buy thicker plywood for your floor, 
    I'm just being lazy here. </p>

    <p>A 150' roll of Tyvek should do it? Maybe two. ${houseWrapCost.toFixed(2)}</p>

    <p>A 225sqft roll of Grace Ice and Water Shield should do it? Maybe two. ${roofingUnderlaymentCost.toFixed(2)}</p>

    <p>Your roof is about {length*width}sqft, a roofing panel is about 30sqft, so you need about
    {(length*width/30).toFixed(1)} of them at ${roofingCost.toFixed(2)}.</p>

    <p>Door and window math is basic, at ${(doorUnitCost+windowsCost).toFixed(2)}</p>
    
    <p>I'm assuming T1-11 siding. I hate T1-11, but it's basic and gets the job done and I had to
        pick something. You need about {(circumference/4).toFixed(1)} pieces at ${sidingCost.toFixed(2)}.
    </p>

    <p>You need about {(circumference/4).toFixed(1)} pieces of drywall at ${drywallCost.toFixed(2)}.</p>

    <p>You need about {length*width}sqft of flooring, at ${flooringCost.toFixed(2)}.</p>

    <p>I got lazy on the electrical costs and just did a small panel and a 250' roll of 12g wire,
        with a box and outlet every 4'. ${electricalCost.toFixed(2)} </p>
        
    <p>For plumbing, I just did a one piece shower unit, an on demand propane water heater that should be
        able to do shower and sink at the same time, and sink. I didn't calculate for PEX or connections. 
        ${plumbingCost.toFixed(2)}
    </p>

    <p>Wow I am not good at shopping for cabinets. I assumed a galley kitchen, so we need two sets. 
        ${cabinetsCost.toFixed(2)}
    </p>

    <p>I'm not getting into the nitty gritty. You need drywall tape and mud, flashing around the 
        roof, flashing around the windows and door, light fixtures, switches... This is supposed to 
        be a ballpark figure, not an exhaustive real plan. </p>

    <p>I didn't account for a loft either. And you'll need a thin mattress for a loft. </p>    
    <p>And then I threw 15% on top, because it's always just a little more than you think it is.</p>

</div>