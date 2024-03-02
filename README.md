# sar-asf-to-gee

<!-- WARNING: THIS FILE WAS AUTOGENERATED! DO NOT EDIT! -->

# Overview

This package facilitates the transfer of ASF HyP3 processed SAR datasets
to Google Cloud Storage and the creation of Google Earth Engine assets.

# Install

## Prerequisites

1.  A NASA Earthdata account
    - You can register for an Earthdata Login here:
      https://www.earthdata.nasa.gov/eosdis/science-system-description/eosdis-components/earthdata-login
    - Create a `.netfc` file that contains the Earthdata authentication
      credentials. See:
      https://nasa-openscapes.github.io/2021-Cloud-Hackathon/tutorials/04_NASA_Earthdata_Authentication.html
2.  A Google Cloud account
    - Create Google Cloud authentication credentials using the [Google
      Cloud Command Line Interface](https://cloud.google.com/cli) with
      the command: `gcloud auth application-default login`
    - See:
      https://cloud.google.com/sdk/gcloud/reference/auth/application-default/login
3.  A Google Earth Engine account
    - Create Google Earth Engine authentication credentials using the
      [Google Earth Engine Command Lin
      Tool](https://developers.google.com/earth-engine/guides/command_line)
      with the command: `earthengine authenticate`
    - See: https://developers.google.com/earth-engine/guides/auth

## Package Installation

Install directly from the GitHub repo.

``` sh
pip install git+https://github.com/gee-community/sar-asf-to-gee.git
```

## How to use

This package assumes that you have already created datasets using the
[ASF HyP3](https://hyp3-docs.asf.alaska.edu/) processing system using
either the [ASF Data Search
Vertex](https://hyp3-docs.asf.alaska.edu/using/vertex/) web interface or
programmatically using the [HyP3 SDK for
Python](https://hyp3-docs.asf.alaska.edu/using/sdk/) or [HyP3 REST
API](https://hyp3-docs.asf.alaska.edu/using/api/).

The [status of previously submitted
jobs](https://search.asf.alaska.edu/#/?searchType=On%20Demand) can found
within the Vertex application.

### Find HyP3 processed datasets

Define a variable for the a job/project name of the HyP3 generated files
that were previously submitted.

``` python
job_name = 'test submit_insar_job'
```

Find HyP3 jobs that are completed.

``` python
from hyp3_sdk import HyP3

hyp3 = HyP3()
batch_completed = hyp3.find_jobs(
    name=job_name,
    status_code='SUCCEEDED',
)
```

### Transfer datasets

Loop through the completed jobs, transferring the results locally, then
to Google Cloud Storage, and then create an Earth Engine asset.

``` python
from sar_asf_to_gee.hyp3 import Transfer

for job in batch_completed:
    t = Transfer(
        job_dict=job.to_dict(),
        gcs_bucket='hyp3-data-staging',
        gee_gcp_project='sar-asf-to-gee',
        gee_image_collection='test_image_collection',
        local_storage='temp_downloads',
    )
    t.hpy3_results_to_local()
    t.to_gcs()
    t.create_gee_asset()
```

    /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/.pixi/env/lib/python3.11/site-packages/asf_search/download/download.py:65: UserWarning: File already exists, skipping download: temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75.zip
      warnings.warn(f'File already exists, skipping download: {os.path.join(path, filename)}')
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_corr.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_corr.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_los_disp.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_los_disp.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_inc_map.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_inc_map.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_vert_disp.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_vert_disp.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_water_mask.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_water_mask.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_lv_phi.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_lv_phi.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_inc_map_ell.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_inc_map_ell.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_lv_theta.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_lv_theta.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_unw_phase.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_unw_phase.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_amp.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_amp.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_wrapped_phase.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_wrapped_phase.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_dem.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_dem.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_water_mask.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_water_mask.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_lv_phi.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_lv_phi.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_vert_disp.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_vert_disp.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_inc_map_ell.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_inc_map_ell.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_corr.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_corr.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_dem.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_dem.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_unw_phase.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_unw_phase.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_amp.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_amp.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_los_disp.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_los_disp.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_wrapped_phase.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_wrapped_phase.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_lv_theta.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_lv_theta.tif
    Reading input: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_inc_map.tif

    Adding overviews...
    Updating dataset tags...
    Writing output to: /Users/tylere/Documents/GitHub/gee-community/sar-asf-to-gee/temp_downloads/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_inc_map.tif

    hyp3-data-staging/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_corr.tif
    hyp3-data-staging/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_los_disp.tif
    hyp3-data-staging/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_inc_map.tif
    hyp3-data-staging/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_vert_disp.tif
    hyp3-data-staging/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_water_mask.tif
    hyp3-data-staging/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_lv_phi.tif
    hyp3-data-staging/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_inc_map_ell.tif
    hyp3-data-staging/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_lv_theta.tif
    hyp3-data-staging/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_unw_phase.tif
    hyp3-data-staging/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_amp.tif
    hyp3-data-staging/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_wrapped_phase.tif
    hyp3-data-staging/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75/S1AA_20230331T140803_20230412T140803_VVP012_INT80_G_ueF_3F75_dem.tif
    hyp3-data-staging/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_water_mask.tif
    hyp3-data-staging/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_lv_phi.tif
    hyp3-data-staging/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_vert_disp.tif
    hyp3-data-staging/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_inc_map_ell.tif
    hyp3-data-staging/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_corr.tif
    hyp3-data-staging/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_dem.tif
    hyp3-data-staging/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_unw_phase.tif
    hyp3-data-staging/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_amp.tif
    hyp3-data-staging/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_los_disp.tif
    hyp3-data-staging/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_wrapped_phase.tif
    hyp3-data-staging/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_lv_theta.tif
    hyp3-data-staging/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264/S1AA_20230327T130015_20230408T130016_VVP012_INT80_G_ueF_F264_inc_map.tif
