Erasure coding pool for RBDs
----------------------------
http://ceph.com/community/new-luminous-erasure-coding-rbd-cephfs/

#### For three node cluster

    ceph osd pool create rbd 16 16
    ceph osd pool application enable rbd rbd
    ceph osd erasure-code-profile set ec-21 k=2 m=1 crush-failure-domain=host
    ceph osd pool create ec21 16 erasure ec-21
    ceph osd pool set ec21 allow_ec_overwrites true
    ceph osd pool application enable ec21 rbd
    rbd create rbd/foo --size 102400 --data-pool ec21

#### For 6+ node cluster
#### Use for calculating pg numbers http://ceph.com/pgcalc/
#### erasure coded pool with 60 osds

    ceph osd pool create rbd 256 256
    ceph osd pool application enable rbd rbd
    ceph osd erasure-code-profile set ec-42 k=4 m=2 crush-failure-domain=host
    ceph osd pool create ec42 1024 erasure ec-42
    ceph osd pool set ec42 allow_ec_overwrites true
    ceph osd pool application enable ec42 rbd

    //Huge Drive
    rbd create rbd/data1 --size 110T --data-pool ec42
    //Benchmarking drive test
    rbd create rbd/foo --size 102400 --data-pool ec42

#### Benchmarking

    See ceph-benchmark.md

#### Mapping

    rbd map foo
    mkfs.xfs /dev/rbd0
    mount /dev/rbd0 /mnt
