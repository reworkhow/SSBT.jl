# SSBR

[![Build Status](https://travis-ci.org/reworkhow/SSBR.jl.svg?branch=master)](https://travis-ci.org/reworkhow/SSBR.jl)

SSBR is a tool for single step Bayesian regression analyses.


####Quick-start

```Julia
using SSBR

ped,A_Mats,numSSBayes = calc_Ai("../simulation/ped.txt","../simulation/genotype.ID")
df    = read_genotypes("../simulation/genotype.txt",numSSBayes)
M_Mats = make_MMats(df,A_Mats,ped)
y_Vecs = make_yVecs("../simulation/phenotype.txt",numSSBayes)
J_Vecs = make_JVecs(numSSBayes,A_Mats)
Z_Mats = make_ZMats(ped,y_Vecs,numSSBayes)
X_Mats, W_Mats = make_XWMats(J_Vecs,Z_Mats,M_Mats,numSSBayes)

#Gibbs sampler
nIter  = 50000
vRes   = 1.0
vG     = 1.0
vAlpha = vG/numSSBayes.num_markers
aHat,alphaHat,betaHat,epsiHat=ssGibbs(M_Mats,y_Vecs,J_Vecs,Z_Mats,X_Mats,W_Mats,A_Mats,numSSBayes,myEP);

#Mixed Model Equation
vRes   = 1.0
vG     = 1.0
vAlpha = vG/numSSBayes.num_markers
aHat2,alphaHat2,betaHat2,epsiHat2=ssMME(M_Mats,y_Vecs,J_Vecs,Z_Mats,X_Mats,W_Mats,A_Mats,numSSBayes);

#check accuracy
df = readtable("../simulation/bv.txt", eltypes =[UTF8String, Float64], separator = ' ',header=false)
a  = Array(Float64,numSSBayes.num_ped)
for (i,ID) in enumerate(df[:,1])
     j = ped.idMap[ID].seqID
     a[j] = df[i,2]
end
cor(a,aHat)

```

####More

* **homepage**: [QTL.rocks](http://QTL.rocks)
* **Installation**: at the Julia REPL, `Pkg.clone("https://github.com/reworkhow/SSBR.jl.git")`
* **Authors**: [Hao Cheng](http://reworkhow.github.io),[Rohan Fernando](http://www.ans.iastate.edu/faculty/index.php?id=rohan)