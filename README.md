# retained-intron-neoantigen-pipeline

This pipeline calls RNA-based neoantigens from intron retention events derived from RNA-Seq data and identified through the KMA package (see run instructions below for further detail on this).

```git clone https://github.com/mskcc/retained-intron-neoantigen-pipeline.git```

To run: 
- Download NetMHCPan-3.0 (http://www.cbs.dtu.dk/cgi-bin/nph-sw_request?netMHCpan) and change paths in runNetMHCpan.py file (line 62).
  - ```bash
    netmhcdir=/opt/common/CentOS_6-dev/netMHCpan/netMHCpan-3.0 # or wherever netMHCpan is installed
    sed -i "s,/xchip/cga_home/margolis/Packages/netMHCPan/netMHCpan-3.0,${netmhcdir},g" retained-intron-neoantigen-pipeline/runNetMHCpan.py      
    ``` 
- Download twoBitToFa utility from UCSC genome browser (https://genome.ucsc.edu/goldenpath/help/twoBit.html) and change paths in kmaToPeptideSeqs.py file (line 173).
  - ```bash
    mkdir twoBitToFa
    cd twoBitToFa
    rsync -aP rsync://hgdownload.soe.ucsc.edu/genome/admin/exe/linux.x86_64/ ./
    twoBitToFa=`readlink -m twoBitToFa`
    sed -i "s,/xchip/cga_home/margolis/Packages/tableBrowser/twoBitToFa,${twoBitToFa},g" ../retained-intron-neoantigen-pipeline/kmaToPeptideSeqs.py
    ```
- Download MySQL (you will use it to query the UCSC table browser via public servers).
- Download and run KMA-kallisto package (https://github.com/pachterlab/kma/blob/master/inst/kma-kallisto.zip). The output from this package (ir$flat file, example files run_kma_example.R and kma_output_file_example.csv in this repo) will be the direct input to this pipeline.
- Change paths in shell script getNeoantigenBinders.sh (notes in file comments).
- Run getNeoantigenBinders.sh from command line as an SGE Array Job. This script is a wrapper and will call all other relevant Python scripts.

Additional notes:
- Detailed execution instructions and functionality descriptions can be found in each script header, as well as for each individual function.
- Feel free to create an Issue if errors arise.
