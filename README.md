# aether-config

### Download aether-in-a-box (aiab) files
	cd; git clone "https://gerrit.opencord.org/aether-in-a-box"
	mkdir -p ~/cord
	cd ~/cord
	git clone "https://gerrit.opencord.org/sdcore-helm-charts"
	

### Download SMF source code

SMF with DB changes: `git clone https://github.com/JingqiHuang/smf.git`

### Build image
	cd <PATH_TO_SMF_FOLDER>/smf
	make docker-build

### Modify configuration files to use our own image
In ~/aether-in-a-box/sd-core-5g-values.yaml, change the configuration for `5g-control-plane` into the following

```
	5g-control-plane:
	  enable5G: true
	  images:
	    # repository: "registry.opennetworking.org/docker.io/"
	    repository: "" #default docker hub
	  tags:
	    init: omecproject/pod-init:1.0.0
	    amf: omecproject/5gc-amf:master-439afd0
	    nrf: omecproject/5gc-nrf:master-5844fcf
	    # smf: omecproject/5gc-smf:master-7ef661b
	    smf: 5gc-smf:0.0.1-dev-dbchanges
	    ausf: omecproject/5gc-ausf:master-7dcc39c
	    nssf: omecproject/5gc-nssf:master-e751140
	    pcf: omecproject/5gc-pcf:master-874bbe1
	    udr: omecproject/5gc-udr:master-3756e35
	    udm: omecproject/5gc-udm:master-15369ab
	    webui: omecproject/5gc-webui:master-727636a
	    sctplb: sctplb:0.0.1-dev-local1
	  pullPolicy: IfNotPresent
```

`smf: 5gc-smf:0.0.1-dev-dbchanges` is to override the image tag and use the image build by our own


### Modify configuration files to choose different control event tests
In the file ~/aether-in-a-box/sd-core-5g-values.yaml, go to the field `5g-ran-sim.config.gnbsim.yamlCfgFiles.gnb.conf.configuration.profiles.

In each profile, profileType sets the control event, ueCount sets the test UE number, and enable sets whether this profile will be run or not. If you want to run certain control event, check out its profile and modify the field ueCount and enable


### Set up aether-in-a-box and run the test
run ```make 5g-test```
