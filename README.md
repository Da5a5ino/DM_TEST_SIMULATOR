<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DM Pro Training Platform</title>
<style>
:root {
 --primary:#0d6efd;
 --bg:#f4f6f9;
 --card:#ffffff;
}
body{font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Arial;background:var(--bg);margin:0}
header{background:var(--primary);color:#fff;padding:14px;text-align:center;font-weight:600}
.container{max-width:1200px;margin:auto;padding:12px}
.main{display:flex;gap:12px}
.left{flex:3;background:var(--card);padding:16px;border-radius:12px}
.right{flex:1;background:var(--card);padding:12px;border-radius:12px;max-height:70vh;overflow:auto}
.option{display:block;background:#eef2f6;margin:6px 0;padding:12px;border-radius:8px;cursor:pointer;font-size:15px}
.selected{background:var(--primary);color:#fff}
.correct{background:#28a745;color:#fff}
.wrong{background:#dc3545;color:#fff}
.progress{height:8px;background:#e0e4e8;border-radius:6px;margin:10px 0}
#bar{height:100%;background:var(--primary);width:0%}
button{padding:10px 14px;margin:4px;border:none;background:var(--primary);color:#fff;border-radius:8px;font-size:14px}
.palette button{width:36px;height:36px;margin:3px;border:none;border-radius:6px;background:#e0e4e8;font-size:12px}
.palette button.answered{background:#198754;color:#fff}
.palette button.wrong{background:#dc3545;color:#fff}
select{padding:10px;border-radius:8px;border:1px solid #ddd;margin-bottom:6px;width:100%}
@media(max-width:900px){
 .main{flex-direction:column}
 .right{max-height:none}
}
</style>
</head>
<body>
<header>DM Professional Training Platform</header>
<div class="container">

<select id="partSelect" onchange="changePart()">
<option value="all">All Parts</option>
</select>

<div class="progress"><div id="bar"></div></div>

<div class="main">
<div class="left">
<div id="question"></div>
<div id="options"></div>
<div>
<button onclick="prevQ()">Prev</button>
<button onclick="nextQ()">Next</button>
<button onclick="submitExam()">Submit</button>
</div>
</div>

<div class="right">
<b>Navigator</b>
<div id="palette" class="palette"></div>
</div>
</div>

<div id="result"></div>

</div>

<script>
// TRAINING PLATFORM — PARTS OF 40 QUESTIONS (NON CUMULATIVE)

const part1 = [
{
question:"Density of Steel",
options:["6000 kg/m3","5800 kg/m3","7800 kg/m3","8500 kg/m3"],
answer:2
},
{
question:"Density of Concrete",
options:["1000 kg/m3","3800 kg/m3","2500 kg/m3","1500 kg/m3"],
answer:2
},
{
question:"If the bolts are located outside the flanges of UC column close to web then it is",
options:["Hinge","Fixed","Sliding","None"],
answer:1
},
{
question:"If the bolts are located between flanges of UC column far from web then this is",
options:["Hinge","Fixed","Sliding","None"],
answer:0
},
{
question:"Common types of slabs",
options:["Ribbed","Flat","Solid","All of the above"],
answer:3
},
{
question:"Beam repair techniques",
options:["Steel plates","All","Carbon fibre laminates","Carbon fibre rods"],
answer:1
},
{
question:"Proper curing should be continued for",
options:["7 days","28 days","10 days","4 days"],
answer:0
},
{
question:"Apex connection is the connection between",
options:["Beam to Column","None","2 Columns","2 Beams"],
answer:3
},
{
question:"Distance between CG of compression and CG of tension flanges for a plate girder is called",
options:["Effective depth","None","Overall depth","Clear depth"],
answer:0
},
{
question:"Minimum distance between bolts",
options:["4d","3.5d","1.5d","2.5d"],
answer:3
},
{
question:"Maximum shoring deflection if spacing between shoring and road less than 5m",
options:["10 mm","20 mm","30 mm","50 mm"],
answer:0
},
{
question:"Soldier piles without tie anchors allowed excavation depth",
options:["8 m","6 m","4 m","5 m"],
answer:3
},
{
question:"Recommended shoring system as per DM circular 141",
options:["Secant","Contiguous","None","Soldier"],
answer:0
},
{
question:"Open excavation angle",
options:["40°","60°","30°","25°"],
answer:0
},
{
question:"Difference between secant and contiguous piles",
options:["Contiguous overlapped","Secant overlapped","Other","All reinforced"],
answer:1
},
{
question:"Water tight shoring system",
options:["Contiguous","Soldier","Other","Secant"],
answer:3
},
{
question:"Percentage of piles tested by static load test",
options:["100%","1%","10%","5%"],
answer:1
},
{
question:"Minimum diameter of micropile",
options:["250 mm","300 mm","400 mm","500 mm"],
answer:0
},
{
question:"Level of shoring RC piles below ground level",
options:["1 m","1.5 m","2 m","None"],
answer:2
},
{
question:"Liquefaction induced effects",
options:["Loss of bearing capacity","All","Land sliding","Loss of lateral stiffness"],
answer:1
},
{
question:"Liquefaction induced effects",
options:["Loss of bearing capacity","Landslide","Settlement","All of the above"],
answer:3
},
{
question:"Cement commonly used for superstructure",
options:["OPC","Blast furnace slag cement","Low heat cement","Alumina cement"],
answer:0
},
{
question:"Density of cement",
options:["1440 kg/m3","1600 kg/m3","2400 kg/m3","800 kg/m3"],
answer:0
},
{
question:"Density of lightweight concrete",
options:["800-2000 kg/m3","2000-2400 kg/m3","400-800 kg/m3","2400-2600 kg/m3"],
answer:0
},
{
question:"Splicing length for pile",
options:["30D","50D","60D","20D"],
answer:1
},
{
question:"Hydration of cement means",
options:["Chemical reaction between cement and water","Reaction with air","Reaction with sand","None"],
answer:0
},
{
question:"Core test is done after",
options:["3 days","14 days","28 days","1 day"],
answer:2
},
{
question:"Cube compressive strength acceptance",
options:[">85% specified strength","<85%","Equal","None"],
answer:0
},
{
question:"Difference between secant and contiguous pile",
options:["Secant overlapped","Contiguous overlapped","All reinforced","None"],
answer:0
},
{
question:"Edge distance for bolt",
options:["2.5d","2d","1.25d","1.5d"],
answer:2
},
{
question:"Eave connection is between",
options:["Beam to beam","Column to column","Column to beam","None"],
answer:2
},
{
question:"Minimum steel percentage in compaction pile",
options:["0.6%","1%","2%","0.8%"],
answer:3
},
{
question:"Shoring methods",
options:["Secant pile","Contiguous pile","Soldier pile","All"],
answer:3
},
{
question:"Ultrasonic pulse velocity done for pile diameter",
options:["800 mm","900 mm","1200 mm","1500 mm"],
answer:2
},
{
question:"Material between H and I beam soldier pile",
options:["Concrete","Timber","Steel","All"],
answer:3
},
{
question:"Internal backfilling layer thickness",
options:["25 cm","40 cm","30 cm","50 cm"],
answer:0
},
{
question:"Admixtures added to increase",
options:["Durability","Decrease permeability","Reduce segregation","All"],
answer:3
},
{
question:"Lowest part of structure",
options:["Footing","Basement","Substructure","Tie beam"],
answer:0
},
{
question:"Most commonly used slab",
options:["Solid slab","Flat slab","Panelled slab","Post tension"],
answer:0
},
{
question:"Types of shoring",
options:["H beam","Contiguous","Secant","Diaphragm wall"],
answer:3
}
];

const part2 = [
{
question:"Shotcrete could be used in",
options:["Shoring walls","Repair works","Slope protection","All"],
answer:3
},
{
question:"Allowable deflection for 5m excavation depth",
options:["10 mm","20 mm","30 mm","50 mm"],
answer:0
},
{
question:"Purpose of concrete curing",
options:["Keep surface wet","Increase strength","Increase workability","All"],
answer:0
},
{
question:"Workability of concrete means",
options:["Ease of placing without segregation","Working stress","Working strain","None"],
answer:0
},
{
question:"Contractor required documents before start",
options:["Permit","NOCs","Soil report","All"],
answer:3
},
{
question:"Density of water",
options:["1500 kg/m3","1000 kg/m3","2500 kg/m3","500 kg/m3"],
answer:1
},
{
question:"Density of wood",
options:["1500 kg/m3","7800 kg/m3","2500 kg/m3","2400 kg/m3"],
answer:0
},
{
question:"Admixtures purpose",
options:["Decrease permeability","Increase durability","Reduce segregation","All"],
answer:3
},
{
question:"Beam side shutters removal",
options:["1 day","7 days","14 days","3.5 days"],
answer:0
},
{
question:"Eave connection between",
options:["Beam to beam","Beam to column","Beam to slab","None"],
answer:1
},
{
question:"Deep compaction technique in Dubai",
options:["Vibro compaction","Soil removal","Grouting","Vertical drains"],
answer:0
},
{
question:"Steel grade for H beam and struts",
options:["355","275","All","None"],
answer:2
},
{
question:"Minimum pile head depth in pile cap",
options:["150 mm","100 mm","50 mm","75 mm"],
answer:0
},
{
question:"Minimum overlap in secant piles",
options:["150 mm","100 mm","50 mm","75 mm"],
answer:2
},
{
question:"Minimum splice overlap for pile reinforcement",
options:["60D","50D","45D","35D"],
answer:1
},
{
question:"Maximum shoring deflection if spacing >5m",
options:["30 mm","50 mm","10 mm","20 mm"],
answer:1
},
{
question:"Soldier pile without anchors max depth",
options:["8 m","6 m","4 m","5 m"],
answer:3
},
{
question:"Max strands per tendon PT slab",
options:["2","3","5","7"],
answer:2
},
{
question:"Recommended shoring system",
options:["Secant","Contiguous","None","Soldier"],
answer:0
},
{
question:"Shoring piles level below GL",
options:["1 m","1.5 m","2 m","None"],
answer:2
},
{
question:"Combo waterproofing CRC has",
options:["Low heat penetration","High reflectance","Keep roof hot","Both a and b"],
answer:3
},
{
question:"Material between H and I beam soldier pile",
options:["Concrete","Timber","Steel","All"],
answer:3
},
{
question:"Piles driven into ground by",
options:["Pile driver","Driller","Vibro machine","None"],
answer:0
},
{
question:"Lower horizontal part of window frame",
options:["Head","Style","Sill","Frame"],
answer:2
},
{
question:"Combo waterproofing approved provides",
options:["Waterproofing","Thermal insulation","Leakage proof","All"],
answer:3
},
{
question:"Incorrect bolt grade",
options:["4.6","10.6","6.6","8.8"],
answer:1
},
{
question:"Minimum reinforcement in compression piles first 6m",
options:["2%","1%","0.6%","0.5%"],
answer:1
},
{
question:"Pile tests required",
options:["Dynamic","Integrity","Sonic","All"],
answer:3
},
{
question:"Cube samples for 110m3 concrete",
options:["12","6","10","3"],
answer:1
},
{
question:"Common type of shoring",
options:["Contiguous","Secant","Soldier","Sheet pile"],
answer:0
},
{
question:"Common type of dewatering",
options:["Deep wells","Well point","Surface","Eductor"],
answer:1
},
{
question:"Material used in I beam shoring",
options:["Timber","Concrete","Steel sheets","All"],
answer:3
},
{
question:"Cement hydration is",
options:["Cement with water","Cement with base","Cement with acid","Cement with chemical"],
answer:0
},
{
question:"Distance between CG tension and compression",
options:["Critical depth","Effective depth","Anchor length","Normal length"],
answer:1
},
{
question:"Ultrasonic test minimum pile diameter",
options:["20 cm","30 cm","100 cm","None"],
answer:2
},
{
question:"Splice length reinforcement",
options:["40D","50D","20D","60D"],
answer:1
},
{
question:"Minimum deflection I beam shoring <5m",
options:["50 mm","20 mm","30 mm","25 mm"],
answer:1
},
{
question:"First layer block masonry",
options:["Solid block","Thermal block","Hollow block","AAC block"],
answer:0
},
{
question:"Water cement ratio",
options:["Water/concrete weight","Water/concrete volume","Water/cement weight","Water/cement volume"],
answer:2
},
{
question:"Core samples taken for",
options:["Tensile strength","Compressive strength","Water absorption","Surface hardness"],
answer:1
}
];

const part3 = [
{
question:"Core samples used for",
options:["Tensile strength","Compressive strength","Water absorption","Surface hardness"],
answer:1
},
{
question:"Shutters and bottom supports removal for slab",
options:["10 days","12 days","14 days","16 days"],
answer:2
},
{
question:"Shotcrete used in",
options:["Repair surface","Dam surface","Decorative surface","All"],
answer:3
},
{
question:"Schmidt hammer test",
options:["Destructive","Non destructive","Chemical test","None"],
answer:1
},
{
question:"First compression test on cubes after",
options:["3 days","5 days","7 days","9 days"],
answer:2
},
{
question:"Density of aluminium",
options:["3500 kg/m3","2700 kg/m3","2500 kg/m3","3000 kg/m3"],
answer:1
},
{
question:"Core samples taken after",
options:["7 days","14 days","21 days","28 days"],
answer:3
},
{
question:"Bitumen used for",
options:["Decorative surface","Shining surface","Smooth surface","Protective surface"],
answer:3
},
{
question:"Modulus of elasticity concrete",
options:["5700√fc","2900","210","4700√fc"],
answer:3
},
{
question:"Poisson ratio steel",
options:["0.3","0.4","0.5","0.6"],
answer:0
},
{
question:"Cement used for slab concrete",
options:["Non shrinkage","SRC","OPC","Rapid hardening"],
answer:2
},
{
question:"Cube samples required for 110m3",
options:["12","6","18","9"],
answer:1
},
{
question:"Adding more water to concrete",
options:["Increase strength","Decrease strength","Same","May vary"],
answer:1
},
{
question:"Average cube strength at 28 days",
options:[">fc by 15%","≥fc","Equal","1.15 fc"],
answer:1
},
{
question:"Density of AAC blocks",
options:["300","400","500","600"],
answer:3
},
{
question:"Maximum deflection piles DM",
options:["5%","4%","2%","1%"],
answer:3
},
{
question:"Max settlement raft foundation",
options:["10 mm","20 mm","30 mm","50 mm"],
answer:3
},
{
question:"Density of steel",
options:["7000","8000","6500","7800"],
answer:3
},
{
question:"Adding water effect",
options:["Workability decrease","Strength increase","Both increase","Workability increase strength decrease"],
answer:3
},
{
question:"Column location concentric footing",
options:["Edge","Corner","Centre","Anywhere"],
answer:2
},
{
question:"Steel quantity in flat slab",
options:["160-185 kg/m3","350-500 kg/m3","50-85 kg/m3","100-120 kg/m3"],
answer:0
},
{
question:"Compacting concrete done to",
options:["Increase strength","Increase durability","Remove air","All"],
answer:3
},
{
question:"Composite piles suitable for",
options:["Max load","Compacting soil","Protect water structure","Above water table"],
answer:2
},
{
question:"Effect of corrosion in steel",
options:["Damage concrete","Cracking","Increase deflection","All"],
answer:3
},
{
question:"Reduce concrete temperature above 32°C",
options:["Add water","Add silica","Add fly ash","Add ice"],
answer:3
},
{
question:"Concrete property",
options:["Strong compression weak tension","Strong tension weak compression","Equal","None"],
answer:0
},
{
question:"Blanket over fresh concrete",
options:["Increase evaporation","Reduce evaporation","Avoid sulphate","Rapid hardening"],
answer:1
},
{
question:"Max deflection isolated footing",
options:["10 mm","15 mm","20 mm","25 mm"],
answer:3
},
{
question:"Density of RCC",
options:["2300","2400","2600","2500"],
answer:3
},
{
question:"Clear cover footing",
options:["65 mm","55 mm","40 mm","75 mm"],
answer:3
},
{
question:"Honeycomb occurs due to",
options:["Improper vibration","Very stiff mix","Less cover","All"],
answer:3
},
{
question:"Building part transferring loads below ground",
options:["Substructure","Superstructure","Foundation","Plinth"],
answer:2
},
{
question:"Most common footing",
options:["Raft","Isolated","Strap","Combined"],
answer:1
},
{
question:"Effective length factor both ends pinned",
options:["0.5","1.0","1.2","1.5"],
answer:1
},
{
question:"Clear cover beams",
options:["10 mm","15 mm","20 mm","25 mm"],
answer:3
},
{
question:"Modulus elasticity steel",
options:["250000","270000","280000","290000"],
answer:3
},
{
question:"Yield strength ASTM A36",
options:["200 MPa","250 MPa","360 MPa","400 MPa"],
answer:1
},
{
question:"Column with large radius gyration",
options:["Easy buckling","Hard buckling","No relation","None"],
answer:1
},
{
question:"Vertical stiffeners spacing plate girder",
options:["d","1.5d","1.75d","2d"],
answer:1
},
{
question:"Poisson ratio concrete",
options:["0.20","0.25","0.30","0.35"],
answer:0
}
];

const part4 = [
{
question:"Poisson ratio steel structural",
options:["0.20","0.25","0.30","0.35"],
answer:2
},
{
question:"Modulus elasticity concrete formula",
options:["5700√fc","4700√fc","2900","210"],
answer:1
},
{
question:"Modulus elasticity steel",
options:["200000","205000","210000","215000"],
answer:1
},
{
question:"Maximum reinforcement beams",
options:["4%","5%","3%","2%"],
answer:0
},
{
question:"Minimum tension reinforcement beams",
options:["0.13%","0.2%","0.3%","0.5%"],
answer:0
},
{
question:"Maximum settlement raft foundation",
options:["10 mm","20 mm","30 mm","50 mm"],
answer:3
},
{
question:"Maximum settlement isolated footing",
options:["10 mm","15 mm","20 mm","25 mm"],
answer:3
},
{
question:"Additional load from side road",
options:["10 kN/m2","15 kN/m2","20 kN/m2","25 kN/m2"],
answer:2
},
{
question:"Additional load from sikka",
options:["10 kN/m2","15 kN/m2","20 kN/m2","25 kN/m2"],
answer:1
},
{
question:"Minimum bolt spacing BS",
options:["2.5d","2d","1.25d","1.5d"],
answer:0
},
{
question:"Max vertical deflection steel BS",
options:["L/180","L/360","L/200","All"],
answer:3
},
{
question:"Allowable strength grade 43 steel",
options:["250","275","355","450"],
answer:1
},
{
question:"Allowable strength grade 50 steel",
options:["250","275","355","450"],
answer:2
},
{
question:"Allowable strength grade 55 steel",
options:["250","275","355","450"],
answer:3
},
{
question:"Minimum steel plate thickness",
options:["4 mm","6 mm","8 mm","10 mm"],
answer:1
},
{
question:"Ultimate deflection steel",
options:["L/240","L/360","H/200","All"],
answer:3
},
{
question:"Single shear capacity bolt 20mm grade 4.6",
options:["29 kN","39.2 kN","50 kN","60 kN"],
answer:1
},
{
question:"Single shear capacity bolt 20mm grade 8.8",
options:["70 kN","80 kN","91.9 kN","100 kN"],
answer:2
},
{
question:"Dynamic analysis required building height",
options:["Above 5 stories","Above 10","Above 3","Above 15"],
answer:0
},
{
question:"Dynamic analysis soil type",
options:["SC","SF","SD","SE"],
answer:1
},
{
question:"Maximum deflection normal floors ACI",
options:["L/360","L/280","L/150","L/180"],
answer:3
},
{
question:"Max deflection long term floors",
options:["L/240","L/480","L/200","L/360"],
answer:0
},
{
question:"Toe depth pile in rock",
options:["3D","4D","2D","D"],
answer:2
},
{
question:"Vertical component earthquake dynamic",
options:["0.5Eh","2/3Eh","Eh","0.25Eh"],
answer:1
},
{
question:"Maximum vertical reinforcement columns",
options:["4%","8%","6%","10%"],
answer:0
},
{
question:"Minimum vertical reinforcement columns",
options:["1%","0.5%","0.8%","0.4%"],
answer:0
},
{
question:"Maximum deflection piles DM",
options:["1%","2%","4%","10%"],
answer:0
},
{
question:"Max deflection piles road <5m",
options:["50 mm","40 mm","30 mm","20 mm"],
answer:0
},
{
question:"Max deflection piles neighbor <5m",
options:["50 mm","40 mm","30 mm","20 mm"],
answer:2
},
{
question:"Minimum distance between piles",
options:["2D","2.5D","3D","4D"],
answer:1
},
{
question:"Minimum distance anchors",
options:["1 m","1.2 m","1.5 m","2 m"],
answer:1
},
{
question:"Minimum anchor length",
options:["2 m","3 m","4 m","5 m"],
answer:1
},
{
question:"Maximum anchor length",
options:["5 m","8 m","10 m","12 m"],
answer:2
},
{
question:"Maximum excavation angle",
options:["30°","40°","50°","60°"],
answer:1
},
{
question:"Horizontal deflection steel column",
options:["H/200","H/250","H/300","H/350"],
answer:2
},
{
question:"Modular ratio concrete",
options:["Ec/Es","Es/Ec","Ec/Ec","None"],
answer:1
},
{
question:"Liquefaction effects",
options:["Settlement","Landslide","Loss bearing","All"],
answer:3
},
{
question:"Integrity test purpose",
options:["Strength","Capacity","Necking detection","Depth"],
answer:2
},
{
question:"Maximum deflection live load floors",
options:["L/360","L/280","L/150","L/180"],
answer:3
},
{
question:"Corrosion effect reinforcement",
options:["Damage","Cracking","More deflection","All"],
answer:3
}
];

const part5 = [
{
question:"Toe depth piles",
options:["3D","4D","2D","D"],
answer:2
},
{
question:"Maximum steel percentage column",
options:["4%","8%","6%","10%"],
answer:0
},
{
question:"Deflection simply supported beam UDL",
options:["5wL4/384EI","wL4/8EI","wL3/48EI","None"],
answer:0
},
{
question:"Grouting in PT tendons used for",
options:["Mechanical connection","Prevent corrosion","Transfer stresses","All"],
answer:3
},
{
question:"PT slab advantages",
options:["Less deflection","Less cracking","Higher load","All"],
answer:3
},
{
question:"Average compressive stress pile concrete",
options:["50%","35%","10%","25%"],
answer:1
},
{
question:"Minimum spiral link diameter piles",
options:["8 mm","10 mm","12 mm","6 mm"],
answer:1
},
{
question:"Slump test measures",
options:["Flowability","Viscosity","Consistency","None"],
answer:2
},
{
question:"Poisson ratio for concrete",
options:["0.10","0.20","0.25","0.30"],
answer:1
},
{
question:"Poisson ratio for structural steel",
options:["0.20","0.25","0.30","0.35"],
answer:2
},
{
question:"Modulus of elasticity Ec for concrete",
options:["4700√fc","5700√fc","2900","210000"],
answer:0
},
{
question:"Modulus of elasticity Es for steel",
options:["200000 N/mm2","205000 N/mm2","210000 N/mm2","215000 N/mm2"],
answer:1
},
{
question:"Maximum reinforcement percentage for beams BS8110",
options:["3%","4%","5%","6%"],
answer:1
},
{
question:"Minimum reinforcement percentage for beams BS8110",
options:["0.8%","0.5%","1%","0.3%"],
answer:0
},
{
question:"Maximum settlement raft foundation",
options:["25 mm","50 mm","75 mm","100 mm"],
answer:1
},
{
question:"Maximum settlement isolated footing",
options:["10 mm","15 mm","20 mm","25 mm"],
answer:3
},
{
question:"Minimum additional load side road DM",
options:["10 kN/m2","15 kN/m2","20 kN/m2","25 kN/m2"],
answer:2
},
{
question:"Minimum additional load side sikka DM",
options:["10 kN/m2","15 kN/m2","20 kN/m2","25 kN/m2"],
answer:1
},
{
question:"Minimum bolt spacing BS",
options:["1.25d","2d","2.5d","3d"],
answer:2
},
{
question:"Maximum vertical deflection brittle finish beams",
options:["L/180","L/200","L/360","L/240"],
answer:2
},
{
question:"Allowable strength grade 43 steel",
options:["250","275","355","450"],
answer:1
},
{
question:"Allowable strength grade 50 steel",
options:["250","275","355","450"],
answer:2
},
{
question:"Allowable strength grade 55 steel",
options:["250","275","355","450"],
answer:3
},
{
question:"Minimum thickness steel plate built up sections",
options:["4 mm","6 mm","8 mm","10 mm"],
answer:1
},
{
question:"Single shear capacity bolt 20mm grade 4.6",
options:["29 kN","39.2 kN","50 kN","60 kN"],
answer:1
},
{
question:"Single shear capacity bolt 20mm grade 8.8",
options:["70 kN","80 kN","91.9 kN","100 kN"],
answer:2
},
{
question:"Modification factor PT slabs DM",
options:["0.25","0.35","0.50","0.70"],
answer:2
},
{
question:"Dynamic analysis required above",
options:["3 stories","5 stories","10 stories","15 stories"],
answer:1
},
{
question:"Response modification factor R for special moment frame",
options:["3","5","8","10"],
answer:2
},
{
question:"Response modification factor R for ordinary moment frame",
options:["3","5","8","10"],
answer:1
},
{
question:"Importance factor I for normal buildings",
options:["1.0","1.25","1.5","2.0"],
answer:0
},
{
question:"Importance factor I for essential buildings",
options:["1.0","1.25","1.5","2.0"],
answer:2
},
{
question:"Base shear depends on",
options:["Mass only","Stiffness only","Mass and stiffness","Height only"],
answer:2
},
{
question:"Fundamental period increases when",
options:["Height increases","Stiffness increases","Mass decreases","Width decreases"],
answer:0
},
{
question:"Building drift limit typical value",
options:["H/100","H/200","H/400","H/800"],
answer:1
},
{
question:"Allowable interstory drift per seismic code",
options:["0.002h","0.004h","0.01h","0.02h"],
answer:1
},
{
question:"Damping ratio for RC structures",
options:["2%","5%","10%","15%"],
answer:1
},
{
question:"Damping ratio for steel structures",
options:["2%","5%","10%","15%"],
answer:1
},
{
question:"Equivalent static method used for",
options:["Low rise regular buildings","Tall buildings","Irregular structures","Bridges"],
answer:0
},
{
question:"Dynamic analysis required when building is",
options:["Regular low rise","Irregular","Short span","Light weight"],
answer:1
}
];

const part6 = [
{
question:"Seismic force increases with",
options:["Decrease in mass","Increase in mass","Decrease in height","Increase stiffness"],
answer:1
},
{
question:"Natural frequency increases when stiffness",
options:["Decreases","Increases","Constant","Zero"],
answer:1
},
{
question:"Soil type with highest amplification",
options:["Rock","Medium soil","Soft soil","Dense sand"],
answer:2
},
{
question:"Base shear formula includes",
options:["Seismic coefficient","Mass","Height","All of the above"],
answer:3
},
{
question:"Seismic load acts primarily",
options:["Vertical","Horizontal","Diagonal","Random"],
answer:1
},
{
question:"Torsion occurs when",
options:["Mass symmetric","Stiffness symmetric","Mass eccentric","Height uniform"],
answer:2
},
{
question:"Higher mode effects important for",
options:["Low rise","Tall buildings","Short span beams","Footings"],
answer:1
},
{
question:"Soft story occurs when stiffness",
options:["Increases suddenly","Decreases suddenly","Constant","Uniform"],
answer:1
},
{
question:"Liquefaction causes",
options:["Increase strength","Loss of bearing capacity","Reduce settlement","Increase stiffness"],
answer:1
},
{
question:"Liquefaction occurs in",
options:["Dense clay","Loose saturated sand","Rock","Dry sand"],
answer:1
},
{
question:"Factor affecting liquefaction",
options:["Water content","Density","Seismic load","All of the above"],
answer:3
},
{
question:"Integrity test used to detect",
options:["Pile strength","Necking or voids","Pile length","Settlement"],
answer:1
},
{
question:"Static load test used to determine",
options:["Capacity","Length","Concrete strength","Pile weight"],
answer:0
},
{
question:"Minimum overlap secant piles",
options:["50 mm","75 mm","100 mm","150 mm"],
answer:0
},
{
question:"Toe depth piles in rock",
options:["D","2D","3D","4D"],
answer:1
},
{
question:"Minimum distance between piles",
options:["2D","2.5D","3D","4D"],
answer:1
},
{
question:"Anchor used to",
options:["Increase stiffness","Reduce lateral movement","Increase load","Support slab"],
answer:1
},
{
question:"Minimum anchor spacing",
options:["1 m","1.2 m","1.5 m","2 m"],
answer:1
},
{
question:"Excavation support system watertight",
options:["Contiguous","Soldier pile","Secant pile","Sheet pile"],
answer:2
},
{
question:"Maximum excavation angle typical",
options:["30°","40°","50°","60°"],
answer:1
},
{
question:"Maximum lateral deflection piles near road",
options:["50 mm","40 mm","30 mm","20 mm"],
answer:0
},
{
question:"Maximum lateral deflection near building",
options:["50 mm","40 mm","30 mm","20 mm"],
answer:2
},
{
question:"Pile capacity depends on",
options:["Soil","Length","Diameter","All"],
answer:3
},
{
question:"Negative skin friction occurs due to",
options:["Soil settlement","Pile uplift","Concrete shrinkage","Seismic load"],
answer:0
},
{
question:"Factor of safety piles typical",
options:["1.5","2","2.5","3"],
answer:2
},
{
question:"Group effect reduces",
options:["Pile length","Pile capacity","Pile diameter","Settlement"],
answer:1
},
{
question:"Settlement of piles depends on",
options:["Load","Soil stiffness","Pile length","All"],
answer:3
},
{
question:"Pile load transferred by",
options:["Skin friction","End bearing","Both","None"],
answer:2
},
{
question:"Maximum deflection normal floors ACI",
options:["L/180","L/240","L/360","L/480"],
answer:2
},
{
question:"Maximum deflection long term floors",
options:["L/240","L/360","L/480","L/600"],
answer:0
},
{
question:"Maximum deflection brittle finishes",
options:["L/180","L/240","L/360","L/480"],
answer:2
},
{
question:"Maximum deflection cantilever beams",
options:["L/120","L/180","L/240","L/360"],
answer:1
},
{
question:"Maximum deflection roof members",
options:["L/120","L/180","L/240","L/360"],
answer:0
},
{
question:"Horizontal drift limit building",
options:["H/100","H/200","H/400","H/800"],
answer:1
},
{
question:"Elastic behavior occurs before",
options:["Cracking","Yielding","Failure","Buckling"],
answer:1
},
{
question:"Plastic behavior occurs after",
options:["Yielding","Cracking","Elastic stage","Initial loading"],
answer:0
},
{
question:"Stiffness defined as",
options:["Force × displacement","Force / displacement","Displacement / force","Stress × strain"],
answer:1
},
{
question:"Higher stiffness results in",
options:["More deflection","Less deflection","More cracking","Less strength"],
answer:1
},
{
question:"Cracking reduces",
options:["Strength","Stiffness","Weight","Density"],
answer:1
},
{
question:"Increase span effect",
options:["Increase stiffness","Increase deflection","Decrease deflection","Increase strength"],
answer:1
}
];

const part7 = [
{
question:"Deflection proportional to",
options:["Load","Span","Elastic modulus","All"],
answer:3
},
{
question:"Increasing inertia reduces",
options:["Deflection","Strength","Load","Span"],
answer:0
},
{
question:"Creep causes",
options:["Instant deflection","Long term deflection","Strength increase","Stiffness increase"],
answer:1
},
{
question:"Shrinkage causes",
options:["Expansion","Cracking","Strength increase","More stiffness"],
answer:1
},
{
question:"Temperature effects cause",
options:["Expansion","Contraction","Stress","All"],
answer:3
},
{
question:"Serviceability limit state relates to",
options:["Deflection","Strength","Collapse","Ultimate load"],
answer:0
},
{
question:"Ultimate limit state relates to",
options:["Serviceability","Collapse","Cracking","Deflection"],
answer:1
},
{
question:"Buckling occurs in",
options:["Compression members","Tension members","Beams only","Slabs"],
answer:0
},
{
question:"Concrete strong in",
options:["Tension","Compression","Shear","Flexure"],
answer:1
},
{
question:"Steel strong in",
options:["Compression","Tension","Both","None"],
answer:2
},
{
question:"Reinforced concrete combines",
options:["Concrete only","Steel only","Steel and concrete","None"],
answer:2
},
{
question:"Factor of safety used to",
options:["Increase load","Reduce risk","Increase strength","Reduce weight"],
answer:1
},
{
question:"Load combinations consider",
options:["Dead load","Live load","Wind load","All"],
answer:3
},
{
question:"Dead load includes",
options:["Self weight","Furniture","People","Wind"],
answer:0
},
{
question:"Live load includes",
options:["Self weight","Occupancy","Concrete","Soil"],
answer:1
},
{
question:"Wind load depends on",
options:["Height","Shape","Exposure","All"],
answer:3
},
{
question:"Seismic load depends on",
options:["Mass","Stiffness","Damping","All"],
answer:3
},
{
question:"Structural analysis used to",
options:["Find loads","Find reactions","Find stresses","All"],
answer:3
},
{
question:"Design aims to satisfy",
options:["Safety","Serviceability","Economy","All"],
answer:3
},
{
question:"Moment equals",
options:["Force × distance","Force / distance","Distance / force","Stress × area"],
answer:0
},
{
question:"Stress equals",
options:["Force × area","Force / area","Area / force","Load × distance"],
answer:1
},
{
question:"Strain equals",
options:["Stress / modulus","Change length / original length","Force / area","Load × length"],
answer:1
},
{
question:"Hooke law valid in",
options:["Elastic range","Plastic range","Failure stage","Cracking stage"],
answer:0
},
{
question:"Neutral axis location depends on",
options:["Material","Section shape","Reinforcement","All"],
answer:3
},
{
question:"Shear failure is",
options:["Brittle","Ductile","Elastic","Plastic"],
answer:0
},
{
question:"Flexural failure preferred because",
options:["Ductile","Brittle","Sudden","Dangerous"],
answer:0
},
{
question:"Over reinforcement leads to",
options:["Ductile failure","Brittle failure","More deflection","Less strength"],
answer:1
},
{
question:"Under reinforcement leads to",
options:["Brittle failure","Ductile failure","Collapse","Buckling"],
answer:1
}
];

const parts = [
part1,
part2,
part3,
part4,
part5,
part6,
part7,
];
const questions = [].concat(...parts);


let allParts = typeof parts !== 'undefined' ? parts : (typeof questions !== 'undefined' ? [questions] : []);

const sel=document.getElementById("partSelect");
allParts.forEach((_,i)=>{
 let opt=document.createElement("option");
 opt.value=i;
 opt.textContent="Part "+(i+1);
 sel.appendChild(opt);
});

let filteredQuestions=[];
let current=0;
let answers=[];
let submitted=false;

function changePart(){
 let v=document.getElementById("partSelect").value;
 if(v==="all") filteredQuestions=[].concat(...allParts);
 else filteredQuestions=allParts[v];
 answers=new Array(filteredQuestions.length).fill(null);
 current=0;
 submitted=false;
 loadQ();
}
changePart();

function loadQ(){
 if(!filteredQuestions.length) return;
 let q=filteredQuestions[current];
 document.getElementById("question").innerHTML=`<h3>${current+1}. ${q.question}</h3>`;
 document.getElementById("options").innerHTML=q.options.map((o,i)=>{
  let cls="option";
  if(submitted){ if(i===q.answer) cls+=" correct"; if(answers[current]===i && i!==q.answer) cls+=" wrong"; }
  else if(answers[current]===i) cls+=" selected";
  return `<div class="${cls}" onclick="selectAns(${i})">${o}</div>`;
 }).join("");
 updatePalette();
 updateBar();
}

function selectAns(i){ if(submitted) return; answers[current]=i; loadQ(); }
function nextQ(){ if(current<filteredQuestions.length-1){current++;loadQ();}}
function prevQ(){ if(current>0){current--;loadQ();}}

function updatePalette(){
 document.getElementById("palette").innerHTML=filteredQuestions.map((q,i)=>{
  let cls="";
  if(answers[i]!=null) cls="answered";
  if(submitted && answers[i]!==q.answer) cls="wrong";
  return `<button class="${cls}" onclick="goQ(${i})">${i+1}</button>`;
 }).join("");
}

function goQ(i){ current=i; loadQ(); }

function updateBar(){
 let percent= filteredQuestions.length ? (answers.filter(a=>a!=null).length/filteredQuestions.length)*100 : 0;
 document.getElementById("bar").style.width=percent+"%";
}

function submitExam(){
 submitted=true;
 let score=0;
 let wrongList=[];
 filteredQuestions.forEach((q,i)=>{
  if(q.answer===answers[i]) score++;
  else wrongList.push({index:i+1,question:q.question,your:(answers[i]!=null?q.options[answers[i]]:"Not answered"),correct:q.options[q.answer]});
 });
 document.getElementById("result").innerHTML=`<h3>Score: ${score}/${filteredQuestions.length} (${Math.round(score/filteredQuestions.length*100)}%)</h3>`;
 renderWrongReport(wrongList);
 loadQ();
}

function renderWrongReport(list){
 let html = `<h3 style="margin-top:20px;color:#dc3545">Wrong Answers Report</h3>`;
 if(list.length===0) html += `<p style="color:green">Perfect Score 🎉</p>`;
 else{
  html += `<div style="background:#fff;padding:15px;border-radius:8px">`;
  list.forEach(item=>{
   html += `<div style="margin-bottom:12px;border-bottom:1px solid #eee;padding-bottom:8px">
   <strong>Q${item.index}:</strong> ${item.question}<br>
   <span style="color:#dc3545">Your answer: ${item.your}</span><br>
   <span style="color:#28a745">Correct answer: ${item.correct}</span>
   </div>`;
  });
  html += `</div>`;
 }
 document.getElementById("result").innerHTML += html;
}
</script>
</body>
</html>
