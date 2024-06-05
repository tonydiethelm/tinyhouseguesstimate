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
Calculate: 
2x4BoardFoot cost = cost of stud / length of stud in feet.
2x6BoardFoot cost = 2x4BoardFoot cost * 6/4
number of studs = (2L+2W)*12/16+4
cost of studs = number of studs*cost of stud
number of floor joists = length*12/16+1
cost of floor joists = number of floor joists*width*2x6BoardFootCost. 
number of roof joists = number of floor joists + 2
cost of roof joists = number of floor joists*(width+2)*2x6BoardFootCost
circumference in feet = length*2+width*2
assume 8' tall walls.
number of wall sheathing ply = circumference /4
number of roof and floor sheathing ply = 2*(length /4)
sheathing ply cost = sheathing cost * (number of wall sheathingy ply + number of roof and wall sheathing ply)
square feet walls = 8*2*length+8*2*width.
total cost of water barrier = cost of water barrier * square feet walls / square feet barrier roll. 
square feet roof = length * 8
total cost of roof barrier = cost of roof barrier * square feet roof/square feet roof barrier roll. 
total cost of roofing = cost of roofing piece * square feet roof/square feet roofing piece.  
total cost of siding = cost of siding piece * square feet siding/square feet siding piece

*/

//basic information
$: totalCost = trailerCost + costOfStuds + costOfFloorJoists + costOfRoofJoists + sheathingPlyCost;
let length = 20;
let width = 8;

//trailer and framing costs
let trailerCost = 4000;
let twoByFourUnitCost = 4;
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
let housewrapUnitCost = 225;


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
Total Cost: ${totalCost}

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
    <p>Cost of your trailer:<input bind:value={trailerCost}></p>
    <p>Cost of a 2x4:<input bind:value={twoByFourUnitCost}></p>
    <p>Cost of sheathing ply:<input bind:value={sheathingPlyUnitCost}></p>

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

</div>