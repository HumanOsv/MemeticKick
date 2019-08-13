# SnippetKick
The SnippetKick algorithm combines local with global search for molecules and clusters. An extension of the Kick program developed by Addicoat et al. is described in which chemically sensible molecular fragments are used in an automated stochastic search algorithm.(1-3)

![alt text](https://github.com/HumanOsv/Logos/blob/master/aguas_kick.png)


(1) 	Addicoat, M. A.; Metha, G. F. Kick: Constraining a Stochastic Search Procedure with Molecular Fragments. J. Comput. Chem. 2009, 30 (1), 57–64.


(2) 	Bera, P. P.; Sattelmeyer, K. W.; Saunders, M.; Iii, H. F. S.; Schleyer, P. R. Mindless Chemistry. Society 2006, 110, 4287–4290.


(3) 	Saunders, M. Stochastic Search for Isomers on a Quantum Mechanical Surface. J. Comput. Chem. 2004, 25 (5), 621–626.


# Getting Started

**1)	Step Zero**

Before starting the installation, it is important to know that SnippetKick is not a full or autonomous software; instead, it needs an assisting program to calculate energies and perform optimizations like Open Babel, Lammps, Gaussian and Mopac. SnippetKick uses these pre-installed tools for the minima local search on the potential energy surface (PES).

**1. Installing external softwares.**

  •	Open Babel (http://openbabel.org/wiki/Category:Installation) (Optional)

  •	Mopac (http://openmopac.net/Download_MOPAC_Executable_Step2.html)

  •	Gaussian (http://gaussian.com/)

  •	Lammps (https://lammps.sandia.gov/download.html#ubuntu) (Optional)
  
  NOTE: mpiexec for Lammps (https://www.mpich.org/static/docs/v3.1/www1/mpiexec.html)

**2. Installing Perl environment.**

Once the programs for both energy calculations and geometry optimizations are working correctly, the Perl environment needs to be installed as well. Perl is a highly capable, feature-rich programming language that runs on many platforms from portable to mainframes.
It can be installed from:
- https://www.perl.org/get.html

There are some additional libraries and softwares that must also be installed to allow SnippetKick to work:

-Install CPAN modules (http://www.cpan.org/modules/INSTALL.html or https://egoleo.wordpress.com/2008/05/19/how-to-install-perl-modules-through-cpan-on-ubuntu-hardy-server/)

    user$ cpan Parallel::ForkManager
      
    user$ cpan Math::Matrix

**2)	Downloading and Installing SnippetKick**

SnippetKick can be directly downloaded as a zip file from the page:

-https://github.com/HumanOsv/SnippetKick

Alternatively, it can be downloaded using the Git tools using the following command:

    user$ git clone https://github.com/HumanOsv/SnippetKick.git

    user$ cd ./SnippetKick

**Note: before downloading using Git tools, make sure to be in your final installation path.**

We recommend to install using Git tools to update future SnippetKick software easily. To update the program, use the following command:

	user$ git pull master
	
Alternatively, SnippetKick could be installed as follows: choose a final installation path, and then extract the ZIP file (containing the software). Provide all the basic permissions for use and, optionally, set SnippetKick.pl file as a system call.

**3)	Running SnippetKick**

To run SnippetKick the following files are necessary in the working directory:

    • Config.in                 : The SnippetKick input file, see below for more information.
    
    • DupGrigoryanSpringborg.pl : The executable file for duplicate molecular fragments.

    • SnippetKick.pl            : The executable file for structure prediction.

    • ReaxFF file (optional)    : Reactive MD-force field file of Lammps.

**Note: SnippetKick.pl can be called from another path if correctly set**

Now, use the following commands to execute this program:

    user$  perl SnippetKick.pl Config.in > out.log

Alternatively, the user can set SnippetKick to run in the background using one of the following methods:

	user$ nohup perl SnippetKick.pl Config.in > out.log
	user$ setsid perl SnippetKick.pl Config.in > out.log

**4)	Input File**

The main input file, known as input.dat, contains all the necessary parameters for a correct calculation. Each variable is explained below.

The Initial Population size (10N,N = Atoms number)

    kick_numb_input = 1000

The size of the box (in Angstroms) length, width, and height.

    box_size = 6.00,6.00,6.00
    
Element symbols and number of atoms for each chemical species. (example: Li 05 or empty)

    chemical_formula = 

**NOTE: Respect the spaces.**

Name of molecular fragments in the simulation.

    fragments = H2O,H2O,H2O

Restricted molecular fragment (YES/NO): 

    fragm_fix = NO

**NOTE: It will always be the first molecular fragment to be placed in keyword fragments.**

Search for duplicate molecular fragments (0=No/1=Yes)

    duplicates = 0

Number of final geometries. 

    kick_numb_output = 10

**NOTE: Species to send for quantum calculations.**


*Configuring the program for chemistry packages*

The number of processors to use in the run (the value may be used to create the input file) and memory to be used in GB.

    core_mem = 4,4

The charge and multiplicity of the candidate.

    charge_multi = 0,1

*Initial geometrical relaxation methods:*

1. Open Babel Force Field.(GAFF,Ghemical,MMFF94,MMFF94s,UFF)

       init_relax = UFF

2. Semiempirical methods of MOPAC.(AUX LARGE PM7).

       init_relax = AUX LARGE PM6

3. Input file ReaxFF Force Field of Lammps.(reaxxFF.Carbon)

       init_relax = reaxxFF.Carbon

4. Execution of only Kick method (empty)

       init_relax = 

Software mopac and gaussian (mopac/gaussian)
     
    software = gaussian

Keywords for gaussian or mopac

*1. Gaussian*

    header = PBE1PBE/SDDAll scf=(maxcycle=512) opt=(cartesian,maxcycle=512)

*2. Mopac*

    header = AUX LARGE PM6

**General Note:** Respect the spaces of separation between the symbol "=".

    Correct : software = gaussian
    Wrong   : software=gaussian

**5) SnippetKick outputs**

After a successful run of the program, several output files will be generated in your working directory.

	Final_coords_gaussian.xyz : Final coordinates XYZ file format of each species ordered less energy at higher energy (gaussian).
	Final_coords_Mopac.xyz    : Final coordinates XYZ file format of each species ordered less energy at higher energy (mopac).
	BOX_kick.vmd              : Shows the box in VMD. (command: vmd -e BOX_kick.vmd)
	out.log                   : Print summary information SnippetKick.
	XYZ_OPT_MM.xyz            : Local optimization Open Babel.
	XYZ_OPT_Mopac.xyz         : Local optimization Mopac.
	XYZ_Coords.xyz            : Structures generated by the Kick Method.
	XYZ_OPT_ReaxFF.xyz        : Local optimization Lammps.
	DuplicatesCoords_GS.xyz   : Duplicates Grigoryan-Springborg Algorithm.
	
	


