# Kcat


# Usage

`kcat -h`:

```
kcat v0.2
usage: /usr/local/bin/kcat [<options>] [<filename>...]

  options:
    -h
    --help  - this help

    -J      - json decode
    -M      - msgpack decode
    -N      - netcdf load
    -P      - py unpickle
    -R      - eval()
    -Y      - yaml decode
    -Z      - zlib decompress

    -j      - json encode
    -m      - msgpack encode
    -n      - netcdf save
    -p      - py pickle
    -r      - repr()
    -y      - yaml encode
    -z      - zlib compress

    --exp=  - run custom expression on buf
              we can use those modules:
                * re - py regexp
                * op - py operator
                * np - numpy        - if installed
                * pd - pandas       - if installed
                * xr - xarray (xray)- if installed
    --str   - convert with str()
              equall to --exp='str(buf)'

    --pretty - encode in pretty format

    --if=   - input file (instead of args)
    --of=   - out file (be careful with multiple --if, last output will be saved)

  filename:
    filename
    -
```

# Examples

generate some data and save as format netCDF:

```bash
echo '' | kcat --exp='xr.DataArray(np.random.randn(2,3,4)).to_dataset(name="R")' -n >a.nc
```

read it back and output:

```bash
kcat --if=a.nc -N --exp='buf.R' --str
```

output:
```
<xarray.DataArray 'R' (dim_0: 2, dim_1: 3, dim_2: 4)>
array([[[-1.40528544,  0.62455772, -1.46824167,  0.58910005],
        [ 1.91870803, -1.39927541,  0.05078447, -0.43398362],
        [-0.94622026,  1.37185607,  0.53048492,  0.65196149]],

       [[ 1.12117453,  1.15023976, -0.7536142 , -1.09189905],
        [ 1.58380027,  2.52831306, -0.8423896 , -1.19814928],
        [ 0.10820024,  0.49066814,  0.35856986, -0.08041386]]])
Coordinates:
  * dim_0    (dim_0) int64 0 1
  * dim_1    (dim_1) int64 0 1 2
  * dim_2    (dim_2) int64 0 1 2 3
```
