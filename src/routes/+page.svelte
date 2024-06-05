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

</style>

<script>
/*

square feet roof = length * 8
total cost of roof barrier = cost of roof barrier * square feet roof/square feet roof barrier roll. 
total cost of roofing = cost of roofing piece * square feet roof/square feet roofing piece.  
total cost of siding = cost of siding piece * square feet siding/square feet siding piece

*/

//basic information
$: totalCost = trailerCost + costOfStuds + costOfFloorJoists + costOfRoofJoists + sheathingPlyCost
+ houseWrapCost + roofingUnderlaymentCost + roofingCost + doorUnitCost + windowsCost;
let length = 20;
let width = 8;

//trailer and framing costs
let trailerCost = 4000;
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
let houseWrapUnitCost = 225;
let houseWrapCost;
if(circumference < 150){
    houseWrapCost = houseWrapUnitCost;
}
if(150 < circumference < 300){
    houseWrapCost = houseWrapUnitCost*2;
}

//roofing
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
let numberOfWindows = 6;
let windowUnitCost = 300;
$: windowsCost = numberOfWindows * windowUnitCost;
let doorUnitCost = 365;


//electrical


//plumbing


//drywall


//flooring


//cabinets


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
        Please fill in the text boxes below and we'll do napkin math as we go along.</p>
    <p>Note! We're are NOT being precise here! 
        This is napkin math! We are making some fairly gross assumptions here. 
        Still, it'll get you in the right ballpark.</p>
    <p>I am providing links to Home Depot to get costs for stuff. 
        Don't take this as an endorsement of Home Depot. I just picked a big box store. 
        I highly recommend you go to someplace with kiln dried lumber. 
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
    <p>Cost of your trailer:<input bind:value={trailerCost}></p>
    <p>Cost of a <a href="https://www.homedepot.com/p/2-in-x-4-in-x-8-ft-Prime-Stud-058449/312528776">2x4</a>:<input bind:value={twoByFourUnitCost}></p>
    <p>Cost of <a href="https://www.homedepot.com/p/15-32-in-x-4-ft-x-8-ft-Sheathing-Plywood-Actual-0-438-in-x-48-in-x-96-in-20159/206827282">sheathing ply</a>:<input bind:value={sheathingPlyUnitCost}></p>
    <p>Cost of 9' by 150' <a href="https://www.homedepot.com/p/TYVEK-9-ft-x-150-ft-HomeWrap-Housewrap-1350-Sq-Ft-D15540826/308793219">housewrap</a>:<input bind:value={houseWrapUnitCost}></p>
    <p>Cost of <a href="https://www.homedepot.com/p/GCP-Applied-Technologies-Grace-Ice-and-Water-Shield-36-in-x-75-ft-Roll-Self-Adhered-Roofing-Underlayment-225-sq-ft-5003002/202088840">225sqft roll of Grace Ice and Water Shield, or equivalent</a>:<input bind:value={graceIceWaterShieldUnitCost}></p>
    <p>number of windows: <input bind:value={numberOfWindows}></p>
    <p>Cost of a window:<input bind:value={windowUnitCost}></p>
    <p>Cost of <a href="https://www.homedepot.com/b/Building-Materials-Roofing-Roof-Panels-Metal-Roofing/10/N-5yc1vZapwhZ1z0sdg9">10' by 3' metal roofing panel</a>:<input bind:value={roofingUnitCost}></p>
    <p>Cost of a <a href="https://www.homedepot.com/p/JELD-WEN-32-in-x-80-in-9-Lite-Primed-Steel-Prehung-Left-Hand-Inswing-Entry-Door-with-Brickmould-735641/202036412">door</a>:<input bind:value={doorUnitCost}></p>

    



</div>

<hr>

<div id='explanations'>
    <h3>Rough Math Explanations/assumptions</h3>
    <p>We're assuming your walls are 8' tall for easy math. They'll be taller.
        You'll need more studs and more sheathing than we're calculating for. 
    </p>
    <p>We're assuming a stud every 16".</p>
    <p>We're assuming a 2x6 costs about 6/4 of a 2x4.</p>

    <p>For {length}' by {width}', assuming 8' tall walls and 16" OC studs, you need about 
    {numberOfStuds} 2x4 studs at about ${costOfStuds}, {numberOfFloorJoists} 2x6 floor joists, and 
    {numberOfRoofJoists} 2x6 roof joists at about ${costOfFloorJoists+costOfRoofJoists}. </p>

    <p>You'll need about {numberOfWallSheathingPly} wall sheathing plywood pieces and about 
        {numberOfRoofAndFloorSheathingPly} number of roof and floor sheathing plywood pieces, 
    which will be about ${sheathingPlyCost}. Honestly, you should buy thicker plywood for your floor, 
    I'm just being lazy here. </p>

    <p>A 150' roll of Tyvek should do it? Maybe two. ${houseWrapCost}</p>

    <p>A 225sqft roll of Grace Ice and Water Shield should do it? Maybe two. ${roofingUnderlaymentCost}</p>

    <p>Your roof is about {length*width}sqft, a roofing panel is about 30sqft, so you need about
    {(length*width/30).toFixed(1)} of them at ${roofingCost.toFixed(2)}.</p>

    <p>And then I threw 15% on top, because it's always just a little more than you think it is.</p>

</div>