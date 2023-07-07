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
UART\_USB
# Ideas

## attach bash, tcl, or sh to flows?
## machine learning front ends
# Tools
<a id="org3fa2a34"></a>
## sailinsc-viewer {plotX|scatter|info target output}
matplotlib.pyplot things from terminal
get basic data summaries
CSV, ETS, Python object storage
<a id="orgf8c3d96"></a>
## sailinsc-importer {output} - ESTRC
convert to trace format
<a id="org57128b7"></a>
## sailinsc-scope {} X
oscilloscope variable defaults to bash variable
run oscilloscope commands
run vxi11 commands
take screenshots
save and load settings scripts
<a id="orgdbfe609"></a>
## sailinsc-board {uart -b binary<sub>number</sub>|uart -h hex<sub>number</sub>|flash bitstream.svf/.rbf}
send serial - With multi type sending
send maybe Ethernet or other IO if need arises
call flash
<a id="orgf9ad621"></a>
## sailinsc-repel {}
get to python REPL with our and library functions 
[https://stackoverflow.com/questions/1395913/how-to-drop-into-repl-read-eval-print-loop-from-python-code](https://stackoverflow.com/questions/1395913/how-to-drop-into-repl-read-eval-print-loop-from-python-code)
