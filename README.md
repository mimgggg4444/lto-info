
https://gdimg.gmarket.co.kr/3746691675/still/600?ver=1714030673

---


LTO(Linear Tape-Open)와 HDD(Hard Disk Drive)를 함께 사용하여 파일 서버를 구축하는 방식은 일반적으로 계층적 스토리지 관리(Hierarchical Storage Management, HSM) 또는 아카이빙 솔루션으로 알려져 있습니다. 이 방식은 자주 액세스되는 데이터는 빠른 HDD에 저장하고, 자주 사용되지 않는 데이터는 cost-effective한 LTO 테이프에 아카이빙하는 것입니다.

귀하의 요구 사항대로 데이터를 LTO와 HDD에 동시에 저장하고, 30일 동안 사용되지 않은 파일을 LTO에만 저장하고 HDD에서는 삭제하는 방식은 타당한 접근 방식입니다. 이 방식의 장점은 다음과 같습니다:

1. 데이터 보존: LTO 테이프는 장기 보존에 적합하며, 데이터 손실 위험을 줄일 수 있습니다.
2. 비용 효율성: LTO 테이프는 HDD에 비해 저렴하므로, 대용량 데이터를 장기간 저장할 때 비용을 절감할 수 있습니다.
3. 스토리지 공간 최적화: 자주 사용되지 않는 파일을 HDD에서 삭제함으로써 스토리지 공간을 효율적으로 관리할 수 있습니다.

다만 이 방식을 구현하기 위해서는 다음과 같은 사항을 고려해야 합니다:

1. HSM 소프트웨어: 데이터 이동 및 관리를 자동화하기 위한 HSM 소프트웨어가 필요합니다.
2. 데이터 액세스 속도: LTO 테이프에서 데이터를 검색하는 속도는 HDD에 비해 느릴 수 있습니다. 따라서 자주 액세스해야 하는 데이터는 HDD에 유지하는 것이 좋습니다.
3. 데이터 동기화: LTO와 HDD 간의 데이터 동기화를 유지하기 위한 메커니즘이 필요합니다.

HSM 솔루션을 구현하기 위해서는 StarWindHSM, Amanda, Bacula 등의 오픈 소스 소프트웨어를 사용하거나, 상용 HSM 소프트웨어를 구매할 수 있습니다.


---

LTO Info Tool
=============

This tool reads the internal memory of LTO/Ultrium cartridges from a tape drive 🖭

More precisely, this tool can:
- Read, decode and display factory and usage information stored in the Cartridge Memory (`CM`) aka Medium Auxiliary Memory (`MAM`)
- View and modify the custom `User Medium Text Label` of a cartridge
- Display basic information about the tape drive device

## :hammer: How to build and use

You need to have a go build environment [properly set up](https://golang.org/doc/install), then just type:

```
make
```

And try the tool by typing:

```
./lto-info
```

By default, the tool will look for a tape device in `/dev/nst0`, or what is pointed to by the `TAPE` environment variable. To specify another device, use the `-d` option.

## :mag: Output example
```
Drive information:
   Vendor  : HP
   Model   : Ultrium 2-SCSI
   Firmware: F63D
Medium information:
  Cartridge Type: 0x00 - Data cartridge
  Medium format : 0x42 - LTO-2
  Formatted as  : 0x42 - LTO-2
  Assign. Org.  : LTO-CVE
  Manufacturer  : IMATION
  Serial No     : 0E00776390
  Manuf. Date   : 2011-12-07 (roughly 9.4 years ago)
  Tape length   : 609 meters
  Tape width    : 12.7 mm
  MAM Capacity  : 4096 bytes (1014 bytes remaining)
Format specs:
   Capacity  :   200 GB native   -   400 GB compressed with a 2:1 ratio
   R/W Speed :    40 MB/s native -    80 MB/s compressed
   Partitions:     1 max partitions supported
   Phy. specs: 4 bands/tape, 16 wraps/band, 8 tracks/wrap, 512 total tracks
   Duration  : 1h23 to fill tape with 64 end-to-end passes (78 seconds/pass)
Usage information:
  Partition space free  : 96% (193746/200448 MiB, 189/195 GiB, 0.18/0.19 TiB)
  Cartridge load count  : 13
  Data written - alltime:        81983 MiB (    80.06 GiB,   0.08 TiB, 0.41 FVE)
  Data read    - alltime:        20674 MiB (    20.19 GiB,   0.02 TiB, 0.10 FVE)
  Data written - session:            0 MiB (     0.00 GiB,   0.00 TiB, 0.00 FVE)
  Data read    - session:          139 MiB (     0.14 GiB,   0.00 TiB, 0.00 FVE)
Previous sessions:
  Session N-0: Used in a device of vendor HP (serial HU10625W0T)
  Session N-1: Used in a device of vendor HP (serial HU10625W0T)
  Session N-2: Used in a device of vendor HP (serial HUP9B067QF)
  Session N-3: Used in a device of vendor HP (serial HUP9B067QF)
```

## :gem: Build-time dependencies

- https://github.com/HewlettPackard/structex: encode/decode bitfields in SCSI structs in a readable way
- https://github.com/benmcclelland/mtio: go bindings for `mt` ioctls
- https://github.com/benmcclelland/sgio: go bindings for `sgio` ioctls
- https://github.com/jessevdk/go-flags: command-line options parser

## :hearts: Thanks

Inspired from other tools written in C:
- https://github.com/arogge/maminfo
- https://github.com/scangeo/lto-cm/

Big up to them!
