# Water_Simulation_GROMACS
Water_Simulation_GROMACS
###************************************#################
steps for simulating water box:
1. Make a topology file named as topol.top and write the following content in it:
	#include "oplsaa.ff/forcefield.itp"
	#include "oplsaa.ff/tip4pew.itp"

	[ System ]
	TIP4PEW

	[ Molecules ]
2. Create a water box:
	gmx solvate -cs tip4p -o water_box.gro -box 2.3 2.3 2.3 -p topol.top
3. Energy minimization
	gmx grompp -f minim.mdp -c water_box.gro -p topol.top -o em.tpr
	gmx mdrun -v -deffnm em
4. NVT-run
	gmx grompp -f nvt.mdp -c em.gro -r em.gro -p topol.top -o nvt.tpr
	gmx mdrun -v -deffnm nvt 
5. NPT-run
	gmx grompp -f npt.mdp -c nvt.gro -r nvt.gro -t nvt.cpt -p topol.top -o npt.tpr
	gmx mdrun -v -deffnm npt
6. Production-run
	gmx grompp -f md.mdp -c npt.gro -t npt.cpt -p topol.top -o md_1.tpr
	gmx mdrun -v -deffnm md_1
