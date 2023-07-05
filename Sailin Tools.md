# Table of Contents
1.  [Ideas](#org238acef)
    1.  [attach bash, tcl, or sh to flows?](#org7fba8e0)
    2.  [machine learning front ends](#orgbea9a73)
2.  [Tools](#org49c1df7)
    1.  [sailinsc-viewer {plot|scatter|info target output}](#org3fa2a34)
    2.  [sailinsc-importer {output}](#orgf8c3d96)
    3.  [sailinsc-scope {}](#org57128b7)
    4.  [sailinsc-board {uart -b binary<sub>number</sub>|uart -h hex<sub>number</sub>|flash bitstream.svf/.rbf}](#orgdbfe609)
    5.  [sailinsc-repel {}](#orgf9ad621)
executable/script style tools
real application/cli/tui
source bash var
dired
integrate with nix
direnv/bash variables:
IP
SCOPE
BUAD
UART<sub>USB</sub>
<a id="org238acef"></a>
# Ideas
<a id="org7fba8e0"></a>
## attach bash, tcl, or sh to flows?
<a id="orgbea9a73"></a>
## machine learning front ends
<a id="org49c1df7"></a>
# Tools
<a id="org3fa2a34"></a>
## sailinsc-viewer {plot|scatter|info target output}
matplotlib.pyplot things from terminal
get basic data summaries
CSV, ETS, Python object storage
<a id="orgf8c3d96"></a>
## sailinsc-importer {output}
convert to trace format
<a id="org57128b7"></a>
## sailinsc-scope {}
oscilloscope variable defaults to bash variable
run oscilloscope commands
run vxi11 commands
take screenshots
save and load settings scripts
<a id="orgdbfe609"></a>
## sailinsc-board {uart -b binary<sub>number</sub>|uart -h hex<sub>number</sub>|flash bitstream.svf/.rbf}
send serial
send maybe Ethernet or other IO if need arises
call flash
<a id="orgf9ad621"></a>
## sailinsc-repel {}
get to python REPL with our and library functions