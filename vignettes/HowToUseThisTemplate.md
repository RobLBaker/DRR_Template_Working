---
title: "How To Use This Template"
date: "7/29/2022"
output:
  html_document:
    df_print: kable
    fig_caption: yes
    dev: svg
    highlight: haddock
    keep_md: yes
    smart: no
    theme: journal
    css: ../common/journalnps.min.css
    toc: yes
    toc_float: yes
    number_sections: yes
  word_document:
    toc: yes
---



# Overview
Data Release Reports (DRRs) are created by the National Park Service and provide detailed descriptions of valuable research datasets, including the methods used to collect the data and technical analyses supporting the quality of the measurements. Data Release Reports focus on helping others reuse data, rather than presenting results, testing hypotheses, or presenting new interpretations, methods or in-depth analyses. 

Data Release Reports are intended to document the processing of fully-QAed data to their final (QCed) form in a reproducible and transparent manner. Data Release Reports document the data collection methods, quality standards, and processing code used to prepare and review data prior to release, and present the quality of resultant data in the context of fitness for their intended use. 

Each DRR cites source and resultant datasets that are published concurrently and cross-referenced. Associated datasets are made publicly available with the exception of data that must be protected from release as per NPS and park-specific policies.

Data packages that are published concurrently with DRRs are intended to be independently citable scientific works that can serve as the basis for subsequent analysis and reporting by NPS or third parties.

# Project Set-up
New projects can be established using this template by downloading a zip file of the package from [This Link](https://github.com/nationalparkservice/IMD_DRR_Template/archive/master.zip)

## Required Packages Not on CRAN

- **EMLassemblyline.** Package developed by EDI.org. As per their github repository, it's designed "for scientists and data managers who need to easily create high quality EML metadata for data publication. EMLassemblyline is a metadata builder that emphasizes auto-extraction of metadata, appends value added content, and inputs user supplied information through common interfaces thereby minimizing user effort while maximizing metadata features for data discovery and reuse."

  ` remotes::install_github("EDIorg/EMLassemblyline")` 

## Folder Structure
General directory contents are as follows (Figure 1):
<div class="figure" style="text-align: center">
<img src="./DirectoryStructure.png" alt="**Figure 1.** Standard project directory structure for data release reports." width="30%" />
<p class="caption">**Figure 1.** Standard project directory structure for data release reports.</p>
</div>

- `common` houses files that are shared across all projects and generally should not be edited by the user. Notable files:

  - `header.html` includes all of the graphic identity material at the top of the rendered report.
  - `footer.html` includes all of the "about this report" information similar to what is found in the front matter of reports published in the NRR and NRDS series.
  - `journalnps.min.css` includes stylesheet tweaks that modify the standard markdown "journal" theme to approxmiate NPS graphic identity standards.
  - `DRR Word Template.docx` is intended for users who prefer to author reports in microsoft word and then convert to markdown (as opposed to authoring reports in rmarkdown). Instructions on how to use this method of DRR report creation are below.
  
- `data` The data folder contains three subfolders for managing datasets used and generated during report production.

  - `raw` is for all source data sets used as the basis for a report. These can either be raw (non-QCed) datasets, or final citable datasets from prior works. The `getDataPackage` function from the `DSTools` package above will download and unpack datapackages into the raw directory.
  - `temp` is intended to house datasets generated during production of the report that are retained for later use in processing (and to help save progress in analyses that require multiple steps). The starting chunk of the template will generate a `reportParameters.RData` dataset of all parameters from the YAML header for later use in the report and associated data packages.
  - `final` is for any final datasets that are to be included in data packages.
  
- `dataPackages` is where the working files for *creating* data packages are held. The folder contains a single `DataPackageTemplate` folder that should be duplicated for each data package to be created. The files in the `DataPackageTemplate` folder are required of the EMLAssemblyline package, and are discussed in further detail below and the separate vignette for creating data packages. Folders and contents are as follows:

  - `data_objects` is for all data files and the manifest (readme) file describing the contents of the data package.
  - `metadata_templates` is a series of files that are either created (or must be edited) before generating the metadata.
  - `eml` which is where the final eml file will end up.
  - `run_EMLassemblylne_for_XX.R` script file, that will take a "final" dataset, create the metadata and manifest files, and zip them up into a data package for publication to the NPS Data Store.

- `figures` contains all the figures for the data release report

- `output` is where the zipped data packages from the script above are written.

# Creating a Reproducible Report
The following is for users who are using the `DRR_Template.RMD` template file to generate a data release report using RMarkdown. If you are planning to author your report in MSWord, please see Appendix A below.

## YAML Header Information
The YAML header contains a series of parameters that are used in the creation of 
the data release report as well as the metadata and manifest files contained in 
associated data packages. The following must be updated prior to finalization

- Parameters
  - `projectDir`. Should point to the root folder on your computer where your template lives.
  - `reportNumber`. This is optional, and should _only_ be included if publishing in the semi-official DRR series. If not, this line can be removed as well as the `subtitle` lines below (lines 23 and 24 in the template)
  - `reportRefID`. This is the Data Store reference ID for the report. Must be here so that it is reused in creation of the metadata and manifest files.
  - `dataPackageXXX`. There are three parameters for each data package associated with each DRR. The RefID parameters should match the Data Store reference IDs; the Title parameter should match the title in Data Store, and the descrption should be a short title/phrase that will be used throughout the text of the report, metadata, and manifest files. Data package parameters can be added (or removed) as needed, but the template includes two by default
  - `title`. The title of the Data Release Report.
  - `author`. Author information should be edited to include the authors of the report. Two examples are included.
  
Because folks will ask...  

1. The abstract is not included in the `abstract` field because if it's there it can't be easily reused in other places. This is our workaround to ensure that the abstract can be consistent across the report and data packages.

2. Yes, we intended to use the `subtitle` field to hold the report number. The rmarkdown template does not have a field/placeholder for report number so (for now) this is our workaround. If you need or want a subtitle, this should be included in the title field similar to how it would be included in a citation.

## Standard Code Chunks
In addition to the report outline and a description of content for each section, the template includes four standard code chunks.

- `setup`. Pretty self explanatory, but there are two snippets for loading packages; the `RRpackages` section is a suite of packages that are used to assist with reproducible reporting. You may not need these for your report, but we have included them as part of the base recommended packages. There is a second snippet for `pkgList` that includes all project-specific packages needed. Add as necessary.

- `LoadData`. Any datasets you need to load can go here. 

- `DataPackages` is the chunk used to read in the scripts for generating the data packages.

- `Listing`. Appendix A, by default is the code listing. This will generate all code used in generating the report and data packages.

- `session-info` is the information about the versions of R and packages used in generating the report.


## Figures
Figures should be inserted using code chunks in all cases so that figure
settings can be set in the chunk header. The chunk header should at a minimum
set the fig.align parameter to “center” and the specify the figure caption
(fig.cap parameter). Inserting figures this way will ensure that the caption is
properly formatted and it will apply copy the caption to the figure’s “alt text”
tag, making it 508-compliant.

For example:

````markdown
```{r figure1, echo=FALSE, fig.align="center", out.width="70%", fig.cap="**Figure 2.** Example general workflow to include in the methods section."}
include_graphics("ProcessingWorkflow.png")
```
````

Results in:

<div class="figure" style="text-align: center">
<img src="ProcessingWorkflow.png" alt="**Figure 2.** Example general workflow to include in the methods section." width="70%" />
<p class="caption">**Figure 2.** Example general workflow to include in the methods section.</p>
</div>

## Tables
Tables should be created using the kable function, with additional formatting
options available via the kableExtra package. Specifying the caption in the
kable function call (as opposed to inline markdown text) will ensure that the
caption is appropriately formatted using the theme stylesheet. The table format
has been designed to mimic the NPS graphic identity standards.

In general, tables should be created with the bootstrap_options of striped,
hover, condensed, and responsive, with full_width set to false.

For example:

````markdown
```{r Table1, echo=FALSE}
c1<-c("Mouse1","Mouse2","Mousen")
c2<-c("Drug treatment","Drug treatment","Drug treatment")
c3<-c("Liver dissection","Liver dissection","Liver dissection")
c4<-c("RNA extraction","RNA extraction","RNA extraction")
c5<-c("RNA-Seq","RNA-Seq","RNA-Seq")
c6<-c("GEOXXXXX","GEOXXXXX","GEOXXXXX")
Table1<-data.frame(c1,c2,c3,c4,c5,c6)

kable(Table1, 
      col.names=c("Subjects","Protocol 1","Protocol 2","Protocol 3","Protocol 4","Data"),
      caption="**Table 1.** Experimental study example Data Records table.") %>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"),full_width=F)
```
````
Results in:

<table class="table table-striped table-hover table-condensed table-responsive" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>**Table 1.** Experimental study example Data Records table.</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Subjects </th>
   <th style="text-align:left;"> Protocol 1 </th>
   <th style="text-align:left;"> Protocol 2 </th>
   <th style="text-align:left;"> Protocol 3 </th>
   <th style="text-align:left;"> Protocol 4 </th>
   <th style="text-align:left;"> Data </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Mouse1 </td>
   <td style="text-align:left;"> Drug treatment </td>
   <td style="text-align:left;"> Liver dissection </td>
   <td style="text-align:left;"> RNA extraction </td>
   <td style="text-align:left;"> RNA-Seq </td>
   <td style="text-align:left;"> GEOXXXXX </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mouse2 </td>
   <td style="text-align:left;"> Drug treatment </td>
   <td style="text-align:left;"> Liver dissection </td>
   <td style="text-align:left;"> RNA extraction </td>
   <td style="text-align:left;"> RNA-Seq </td>
   <td style="text-align:left;"> GEOXXXXX </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mousen </td>
   <td style="text-align:left;"> Drug treatment </td>
   <td style="text-align:left;"> Liver dissection </td>
   <td style="text-align:left;"> RNA extraction </td>
   <td style="text-align:left;"> RNA-Seq </td>
   <td style="text-align:left;"> GEOXXXXX </td>
  </tr>
</tbody>
</table>
Note that rendered tables are not fully 508 compliant, but they’re close. We are
currently working on extending (or contributing to) the kable package so that
rendered tables are 508 compliant.

# Creating Data Packages


# Publishing Data Release Reports and Associated Data

## Acquiring Report Numbers
Because data release reports and associated data packages are cross-referential, report
numbers are typically assigned early in data processing and quality evaluation. 

- **Data Store Reference Numbers.** When developing a report and data packages, Data Store references should be created as early in the process as practicable. While the report and data packages are in development, these should not be activated. 

- **Report Numbers.** If you are plannning to publish a data release report with an official DRR number, please contact the IMD Deputy Chief with the Data Store reference number associated with the DRR.  

- **Persistent Identifiers.** IMD is currently pursuing the ability to assign digital object identifiers (DOIs) to scientific works produced by IMD. At that point DOIs will be assigned to all DRRs and concurrently-published data packages will be assigned dois. DOIs will ultimately each resolve to a Data Store Reference; in the meantime, it is recommended that the URL for the Data Store reference be used in lieu of a DOI when citing DRRs and associated data packages. 

## Publishing Draft Data Packages
Once data packages are ready for review, the current best practice is to a) activate them, but b) indicate in the reference metadata that the dataset is provisional with limited access while in review, and c) limit access to specific people (typically the report author and reviewers). This is typically required to conduct the data package usability review.

## Liability Statements ##
Under no circumstances should reports and associated data packages or metadata published in the DRR series contain disclaimers or text that suggests that the work does not meet scientific integrity or information quality standards of the National Park Service. The following disclaimers are suitable for use, depending on whether the data are provisional or final (or approved or certified).

> **For approved & published data sets:** "Unless otherwise stated, all data, metadata and related materials are considered to satisfy the quality standards relative to the purpose for which the data were collected. Although these data and associated metadata have been reviewed for accuracy and completeness and approved for release by the National Park Service Inventory and Monitoring Division, no warranty expressed or implied is made regarding the display or utility of the data for other purposes, nor on all computer systems, nor shall the act of distribution constitute any such warranty." 

> **For provisional data:** "The data you have secured from the National Park Service (NPS) database identified as [database name] have not received approval for release by the NPS Inventory and Monitoring Division, and as such are provisional and subject to revision. The data are released on the condition that neither the NPS nor the U.S. Government shall be held liable for any damages resulting from its authorized or unauthorized use."