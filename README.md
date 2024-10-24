# Copernicus-STAC-API : Sentinel-1-Workflow
This workflow automates the retrieval and processing of Sentinel-1 SAR imagery from the Copernicus DataSpace. It uses Keycloak for secure authentication to download products via the OData API and converts the TIFF files of HH and HV polarizations into NetCDF format. The workflow simplifies access to satellite data for scientific analysis.

# Sentinel-1 SAR Data Workflow using Copernicus APIs

This repository provides a Python-based workflow for downloading, processing, and converting **Sentinel-1 SAR imagery** from the **Copernicus DataSpace** into **NetCDF** format. The workflow integrates with the **Copernicus DataSpace APIs** using **Keycloak authentication** for secure access and utilizes various Python libraries like `requests`, `rasterio`, and `xarray` for data handling.

## Keycloak Authentication

The workflow begins by obtaining an authentication token from the **Keycloak OAuth2 API**. This token is necessary for secure access to the Sentinel-1 product download API.

The function `get_keycloak` sends a POST request to the Keycloak server, providing the `username` and `password` to obtain an OAuth2 token. This token is used for all subsequent API requests to ensure authenticated access.

## Downloading Sentinel-1 Products

Once authenticated, the workflow downloads Sentinel-1 products using the **OData API**. After retrieving the authentication token, the product download link is constructed using the product ID, which is obtained from the API metadata.

### Workflow:
- **Keycloak Authentication**: First, the `get_keycloak` function is called to retrieve the token for secure authorization.
- **Constructing the Download URL**: The product ID is used to build the download URL for accessing the product ZIP file from the OData API.
- **Downloading the Product**: The product is downloaded by passing the authentication token via an HTTP GET request and saved locally for further processing.

## Unzipping and Converting to NetCDF

After downloading the `.zip` files containing Sentinel-1 data, the `.tiff` files representing the `HH` and `HV` polarization channels are extracted and processed. These TIFF files are then converted to **NetCDF** format using the Python libraries `rasterio` and `xarray`.

### Steps:
- **Unzipping the Data**: The `.zip` archive is extracted, and the `.tiff` files within the `measurement` folder are located, specifically those related to `HH` and `HV` polarizations.
- **Converting to NetCDF**: The `.tiff` images are read using `rasterio`, transformed into an `xarray.DataArray`, and appended to a new or existing **NetCDF** file for each product.

## API Overview

### 1. **Keycloak API for Authentication**:
- **URL**: `https://identity.dataspace.copernicus.eu/auth/realms/CDSE/protocol/openid-connect/token`
- **Method**: `POST`
- **Purpose**: To authenticate and obtain an OAuth2 token for accessing Copernicus DataSpace APIs.

### 2. **OData API for Product Download**:
- **URL Template**: `https://zipper.dataspace.copernicus.eu/odata/v1/Products({product_id})/$value`
- **Method**: `GET`
- **Purpose**: Download Sentinel-1 SAR products by their unique product ID.

### 3. **STAC API for Metadata Search (Implied)**:
- **Purpose**: Search for Sentinel-1 SAR imagery based on certain filters like bounding box, time range, and polarization type.

The STAC API is used for finding the Sentinel-1 products that match specific search criteria, which helps to locate product IDs used for downloading the data.

## Summary

This workflow automates the process of downloading and converting Sentinel-1 SAR imagery from the Copernicus DataSpace into a manageable format (NetCDF). It leverages secure authentication via the Keycloak API and processes large satellite imagery efficiently by transforming the raw `.tiff` images into NetCDF format for further scientific analysis.
