# Numpy函数

## numpy.where()
### 给定一个array，找其中某一个值的index：
    import numpy as np
    lbd_min = 1.51
    lbd_max = 1.59
    lbd_target = 1.55
    lbd_step = 10**-6
    lbd0 = np.round(np.arrange(lbd_min, lbd_max, lbd_step), 6)    # 用round是为了避免精度问题
    lbd_target_index = np.where(lbd0 == lbd_target)[0][0]         # np.where()返回的是tuple,第一个[0]取tuple的array，第二个[0]取array的数值。
    print(lbd0[lbd_target_index])
    
### 数据操作问题
```python
>>> a = [1, 2]
>>> a
>>> [1, 2]
>>> b = a
>>> b[1] = 1
>>> b
>>> [1, 1]
>>> a
>>> [1, 1]
```
### silvaco
# MOD WG simulation 
# p&p+ implantation located on the left, and n&n+ on the right


## -----------------------------------------
## Section 1: Device Structure Simulation
## -----------------------------------------

go athena

# top N implant depth
set Ys = 0.18 
set OffsetN=0.00

## --------------------------------------
## Define the device dimension
## --------------------------------------

## definition along X
# Ridge width
set Wr=0.5
# The distance from p right edge to ridge center
set Woffset=0.06
# distance from p doping to n doping
set Wi=0.00
# distance from ridge edge to n doping
set Dln=0.12
# distance from ridge edge to p doping
set Dlp=0.12
# distance from ridge edge to high doping
set Dh=1
# distance from ridge edge to contact
set Dc=1.4
# contact hole width, must be integer times of 0.5
set Wc=0.2

# definition along Y
# Ridge height
set Tr=0.22
# slab height
set Ts=0.09
# BOX thickness
set Tbox=3
# ILD oxide thickness
set Tild=2.5
# passivation oxide thickenss
set Tpaso=0
# passivation nitride thickness
set Tpasn=0
# metal Al thickness
set Tmet=0.6

## --------------------------------------
## screen oxide thickness
## --------------------------------------
SET HM_ox_thick = 0.035
SET PAD_ox_thick = 0.03

## ---------------------------------------
## Define the implant condition
## ---------------------------------------

# Right n type implant setting
set n_dose = 1.0e13
DEFINE N_implant_1 bca n.ion=100000 damage phosphor dose=3e12 energy=90 tilt=0 rotation=0 print.mom
DEFINE N_implant_2 bca n.ion=100000 damage phosphor dose=5e12 energy=25 tilt=0 rotation=0 print.mom

# Left p type implant setting
set p_dose = 9.0e12
DEFINE P_implant_1 bca n.ion=100000 damage boron dose=1.5e13 energy=50 tilt=0 rotation=0 print.mom
# DEFINE P_implant_2 bca n.ion=100000 damage boron dose=2.0e13 energy=32 tilt=0 rotation=0 print.mom

# p+ implant setting
DEFINE PP_implant_1 bca n.ion=100000 damage boron dose=3.5e13 energy=35 tilt=0 rotation=0 print.mom
DEFINE PP_implant_2 bca n.ion=100000 damage boron dose=3.5e13 energy=50 tilt=0 rotation=0 print.mom

# n+ implant seting
DEFINE NN_implant_1 bca n.ion=100000 damage phosphor dose=4e13 energy=55 tilt=0 rotation=0 print.mom
DEFINE NN_implant_2 bca n.ion=100000 damage phosphor dose=4e13 energy=105 tilt=0 rotation=0 print.mom

## -----------------------------------------------
## Define the cluster paramters
## -----------------------------------------------
DEFINE Phos_Cluster i.phos silicon min.cluster=1.0e17 max.cluster=1.0e19 clust.fac=1.4
DEFINE Boron_Cluster i.boron silicon min.cluster=1.0e15 max.cluster=1.0e19 clust.fac=1.4 tau.311.0=8.33e-16 tau.311.e=-3.6


## -----------------------------------------------
## turn on the interstital trap model
## -----------------------------------------------
TRAP silicon total=5.0e17 frac.0=0.5 frac.e=0.0 enable

## -----------------------------------------------
## Interstitial and vacancy parameters
## -----------------------------------------------
DISLOC.LOOP min.loop.co=1e16 max.loop.co=1e18 i.phos silicon
DISLOC.LOOP min.loop.co=1e16 max.loop.co=1e18 i.boron silicon
INTERSTITIAL silicon damalpha=1.0e8 /oxide ksurf.0=1.76e-4

## ------------------------------------------------
## Define anneal conditions
##-------------------------------------------------
DEFINE anneal_rampup t.start=740 t.final=1030 time=5 seconds f.n2=15
DEFINE anneal_rampdown t.start=1030 t.final=740 time=6 seconds f.n2=90
DEFINE anneal temp=1030 time=5 seconds f.n2=15

## --------------------------------------------------
## bias conditions
## --------------------------------------------------
set Bias_First = 0
set ReverseBias_Last  = - 5
set ReverseBias_Step  = -0.25
set ForwardBias_Step = 0.25
set ForwardBias_Last = 0.0


# ------------------------------------------------
# Define the Optical simulation area size 
# ------------------------------------------------
set YMAX=0
set YMIN=-$Tr
set XMIN=-$Wr/2-$Dh-0.5
set XMAX=$Wr/2+$Dh+0.5

set X1 = $Wr/2
set X2 = $Wr/2+$Dh
set X3 = $Wr/2+$Dc
set X4 = $Wr/2+$Dc+$Wc
set X_nplus_left  = $Wr/2+$Dln
set X_pplus_right =-$Wr/2-$Dlp

line x loc=0    spac=0.01
line x loc=$X1  spac=0.02
line x loc=$X2  spac=0.2
line x loc=$X3  spac=0.2
line x loc=$X4  spac=0.2
#line x loc=$Ws  spac=0.2

line y loc=0    spac=0.005
line y loc=0.1  spac=0.1
line y loc=$Tbox    spac=0.5

## ---------------------------------
## process simulation method
## ---------------------------------
METHOD newton full.cpl cluster.dam clust.trans i.loop.sink pdose.loss bdose.loss back=6

MATERIAL silicon no.flip
MATERIAL oxide no.flip

## -----------------------------------
##  doping loss parameters
## ------------------------------------
IMPURITY i.boron silicon /oxide trndl.0=0.0166 trndl.e=0.486 cluster.act act.factor=0.95
IMPURITY i.phosphorus silicon /oxide seg.0=30 trn.0=1.66e-7 trndl.0=0.715 trndl.e=1.75 cluster.act act.factor=0.95


## -------------------------------------
## initialize SOI sub
## -------------------------------------
INIT oxide 
DEPOSIT silicon thick=$Tr c.boron=1.3e15 c.interst=1.0e12 div=$Tr/0.005
STRUCTURE mirror left

#STRUCTURE outfile=1_SOI.str
#TONYPLOT 1_SOI.str

## -----------------------------------------
## HM Depositon and WG etch
## -----------------------------------------
DEPOSIT oxide thick=$HM_ox_thick div=5

## ----------------------------------
## Slab left p-plus implant step
## ----------------------------------
DEPOSIT photo thick=1.0 div=5
ETCH photo left x=$X_pplus_right 
CLUSTER $Boron_Cluster
IMPLANT $PP_implant_1
IMPLANT $PP_implant_2
STRIP photo

#STRUCTURE outfile=6_PPDoping.str
#TONYPLOT 6_PPDoping.str

## ----------------------------------
## Slab right n-plus implant step
## ----------------------------------
DEPOSIT photo thick=1.0 div=5
ETCH photo right x=$X_nplus_left
CLUSTER $Phos_Cluster
IMPLANT $NN_implant_1
IMPLANT $NN_implant_2
STRIP photo


DEPOSIT photo thick=0.8 div=4
ETCH photo left x=-($X1)
ETCH photo right x=$X1
ETCH oxide thick=$HM_ox_thick
ETCH silicon thick=$Tr-$Ts dry
STRIP photo


#STRUCTURE outfile=2_HM.str
#TONYPLOT 2_HM.str

STRIP oxide

#STRUCTURE outfile=3_WG.str
#TONYPLOT 3_WG.str

## ---------------------------------
## Slab right P-type implant step
## ---------------------------------
DEPOSIT oxide thick=$PAD_ox_thick div=5
DEPOSIT photo thick=1.0 div=5
ETCH photo left p1.x=$Woffset
CLUSTER $Boron_Cluster
IMPLANT $P_implant_1
STRIP photo



## ----------------------------------
## Slab left N-type implant
## ----------------------------------
DEPOSIT photo thick=1.0 div=5
ETCH photo right p1.x=-$Wr/2+$OffsetN
CLUSTER $Phos_Cluster
IMPLANT $N_implant_2
IMPLANT $N_implant_1
STRIP photo

#STRUCTURE outfile=4_PDoping.str
#TONYPLOT 4_PDoping.str



#STRUCTURE outfile=5_NDoping.str
#TONYPLOT 5_NDoping.str


#STRUCTURE outfile=7_NNDoping.str
#TONYPLOT 7_NNDoping.str

# RTA 1030C 5s Anneal
#DIFFUSE $anneal_rampup
DIFFUSE $anneal
#DIFFUSE $anneal_rampdown

STRIP oxide

# #STRUCTURE outfile=8_RTA.str
# #TONYPLOT 8_RTA.str

# ILD oxide
DEPOSIT oxide thick=$Tild div=50

# contact hole opening
DEPOSIT photo thick=0.55 div=50
ETCH photo start x=$X3 y=-4
ETCH cont x=$X3 y=1
ETCH cont x=$X4 y=1
ETCH done x=$X4 y=-4
ETCH photo start x=-$X3 y=-4
ETCH cont x=-$X3 y=1
ETCH cont x=-$X4 y=1
ETCH done x=-$X4 y=-4
ETCH oxide thick=$Tild+0.01 dry
STRIP photo

# metallization
DEPOSIT alum thick=$Tmet div=50
ETCH alum start x=-$X3+0.2 y=-5
ETCH cont x=-$X3+0.2 y=1
ETCH cont x=$X3-0.2 y=1
ETCH done x=$X3-0.2 y=-5

ELECTRODE name=anode x=-$X3
ELECTRODE name=cathode x=$X3

STRUCTURE outfile=9_Final.str
# TONYPLOT 9_Final.str

# ------------------------------------------------------------------
# Section 2: extract the sheet resistance
# ------------------------------------------------------------------
system del Rs.txt
extract name="P+_Si_Slab"  sheet.res material="Silicon" mat.occno=1 x.val=0.5*(-$X2+$X_pplus_right) region.occno=1 datafile="Rs.txt"
extract name="P_Si_Slab"  sheet.res material="Silicon" mat.occno=1 x.val=0.5*(-$X1+$X_pplus_right) region.occno=1 datafile="Rs.txt"

# next line modified by yongbo 20130918 x.val from -0.5*($X1)+0.5*$Woffset to -0.5*($X1)+0.5*$Woffset+0.5*$Wi; N means topN
extract name="P_Si_Rib" sheet.res material="Silicon" mat.occno=1 x.val=-0.5*($X1)+0.5*$Woffset region.occno=2 datafile="Rs.txt"
extract name="N_Si_Rib" sheet.res material="Silicon" mat.occno=1 x.val=0.5*$X1+0.5*$Woffset+0.5*$Wi    region.occno=1 datafile="Rs.txt"
extract name="N_Si_Surface" sheet.res material="Silicon" mat.occno=1 x.val=-0.5*($X1+$Woffset) region.occno=1 datafile="Rs.txt"


extract name="N_Si_Slab" sheet.res material="Silicon" mat.occno=1 x.val=0.5*($X1+$X_nplus_left) region.occno=1 datafile="Rs.txt"
extract name="N+_Si_Slab" sheet.res material="Silicon" mat.occno=1 x.val=0.5*($X_nplus_left+$X2) region.occno=1 datafile="Rs.txt"

## calculate the series resistance [ohm.mm] (excluding the n++ and p++ contact regions) Rs_P/topN_N from ($Wr/2+$Woffset) to ($Wr/2+$Woffset+$Wi) 
## Rs_N/topN_N from ($Wr/2-$Woffset) to ($Wr/2-$Woffset-$Wi) modified by yongbo 20130918 
extract name="Rs_P+"  $"P+_Si_Slab" * ($X_pplus_right - (-$X2) )/1000  datafile="Rs.txt"
extract name="Rs_P"   $"P_Si_Slab" * (-$X1 - $X_pplus_right) / 1000 datafile="Rs.txt"
extract name="Rs_P_Rib" $"P_Si_Rib" * ($Wr/2+$Woffset)/1000 datafile="Rs.txt"
extract name="Rs_N_Rib"  $"N_Si_Rib" *($Wr/2-$Woffset-$Wi)/1000 datafile="Rs.txt"
extract name="Rs_N" $"N_Si_Slab" *($X_nplus_left-$X1)/1000 datafile="Rs.txt"
extract name="Rs_N_Sur" $"N_Si_Surface" *($Woffset+$X1)/1000 datafile="Rs.txt"
extract name="Rs_N+" $"N+_Si_Slab" *($X2 - $X_nplus_left)/1000 datafile="Rs.txt"
extract name="Rs_total" $"Rs_P+" + $"Rs_P" + $"Rs_P_Rib" + ($"Rs_N_Rib" * $"Rs_N_Sur")/($"Rs_N_Rib" + "Rs_N_Sur") + $"Rs_N"  + $"Rs_N+" datafile="Rs.txt"

## -----------------------------------------------------------------
## Section 3: Using the atlas to extract elec/hole 2D distribution
## -----------------------------------------------------------------

go atlas
set Tild=2.5
set Bias_First = 0
set ReverseBias_Last  = - 5
set ReverseBias_Step  = -0.25
set ForwardBias_Step = 0.25
set ForwardBias_Last = 0.0

init infile=9_Final.str

set ymin0=-$Tild/2-0.5
set ymax0=$Tild/2+0.5
set xmin0=-1.5
set xmax0=1.5

lx.mesh node=1 location=$xmin0
lx.mesh node=100 location=$xmax0
ly.mesh node=1 location=$ymin0
ly.mesh node=100 location=$ymax0

ELECTRODE name=anode ^helm
ELECTRODE name=cathode ^helm


# ================================================================
#  material props.
# ================================================================

# parametrs for free-carrier absorption model ABS.FCARRIER, 
# given by: 
# R.Soren, B. Benett, IEEE J of QE, v 23, n. 1 , 1987, pp. 123-129
# R.Soren, B. Benett, IEEE J of QE, v 23, n. 12 , 1987, pp. 2159-2166
#
# delta_refraction= -(FC.RN*n^FC.EXPRN+FC.RP*p^FC.EXPRP)
# delta_absorption=  (FC.AN*n^FC.EXPAN+FC.AP*p^FC.EXPAP)
#
# These parameters are default for Si, Ge, Polysilicon
material  material=silicon  taup0=1.0e-4  taun0=1.0e-4 fc.an=8.88e-21 fc.ap=5.84e-20 fc.expan=1.167 fc.expap=1.109
material  material=silicon  fc.rn=5.4e-22 fc.rp=1.53e-18 fc.exprn=1.011 fc.exprp=0.838

# ================================================================
# interfaces 
# ================================================================
interface s.n=100 s.p=100
interface s.n=20 s.p=20  y.min=-0.01  y.max=0.01

# ==============================================================
# models definition :very important    
# ==============================================================

# consrh: Shockley-Read-Hall recombination
# conmob: concentration dependent mobility model used for silicon and gallium arsenide. Valid for 300K only
# bgn:  band-gap narrowing
# fermi: Fermi-Dirac carrier statistics 
# abs.fcarrier: free-carrier absorption model
models fermi consrh auger conmob fldmob bgn kla abs.fcarrier


solve init
output con.band val.band band.param
save  outfile=Add_Ver_NPJ00.str master
# tonyplot Add_Ver_NPJ00.str

method   newton trap climit=1e-4
output   refr.index wvgd.refr flowlines con.band val.band recomb u.srh u.aug u.rad
# log outf=Add_Ver_NPJ.log master

solve init

#  TRACE parameter on the WAVEGUIDE statement means that Helmholtz eq-n
#  will be solved for each bias

waveguide  trace v.helm  wavelength=1.55   nmode=1  ly.min=$ymin0 ly.max=$ymax0 lx.min=$xmin0 lx.max=$xmax0 index.model=1

solve prev
save outfile = spatial_profile.str
# tonyplot spatial_profile.str

log outfile = MOD_PN_DC.log master
solve Vanode= $ReverseBias_Last Vstep=$ForwardBias_Step Vfinal=$ForwardBias_Last name=anode 
log off
# tonyplot MOD_PN_DC.log


## AC calculation

# reverse bias to min
# log	  outf= MOD_PN_AC.log master
# solve	  init
# solve    Vanode=$Bias_First Vstep=$ReverseBias_Step Vfinal=$ReverseBias_Last name=anode ac freq=1.0e6
##tonyplot MOD_PN_AC.log

solve init
## forward bias to max
log	  outf= MOD_PN_AC.log master
solve    Vanode=$ReverseBias_Last Vstep=$ForwardBias_Step Vfinal=$ForwardBias_Last name=anode ac freq= 1.0e6
tonyplot spatial_profile.str

quit


go athena
init infile="9_Final.str"
tonyplot 9_Final.str
extract name="SIMS" ave(curve(depth,impurity="Boron" material="Silicon" mat.occno=1 x.val=-0.1)) outfile="p_SIMS.dat"
extract name="SIMS" ave(curve(depth,impurity="Phosphorus" material="Silicon" mat.occno=1 x.val=-0.1)) outfile="n_SIMS.dat"
extract name="SIMS" ave(curve(depth,impurity="Net Doping" material="Silicon" mat.occno=1 x.val=-0.1)) outfile="net_SIMS.dat"
