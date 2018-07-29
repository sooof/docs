---
name: BTS配置
sort: 1
---

# BTS配置

配置OpenBTS
配置OpenBTS：

将app/下的openbts.example.config复制为openbts.config，通过修改openbts.config相关参数配置OpenBTS。

A．    设置PLMN（MCC+MNC），MCC为国家号，例如中国为460，MNC为网络号，例如中国移动为01和02。最好将OpenBTS的PLMN设置为手机prefer PLMN 列表中的PLMN号，使手机能够更为顺利找到OpenBTS。此处我们设置为460-02，为中国移动在1800MHz频段备用的PLMN号，一般国内的手机都将此PLMN号列入prefer PLMN列表。（后来我们一直用001 01这个号，是一个测试用的PLMN号，保证不会与运营商的网络冲突。）

B．    设置工作频段，此处我们使用900MHz频段，将频段设为900MHz。

C．    设置ARFCN，需要将OpenBTS的工作频点（ARFCN）设置为当前空闲的频点，通过usrp_fft.py扫描900MHz下行频段，找到空闲的频点（f）。900MHz下行频段的ARFCN可以通过公式ARFCN=(f-935)/0.2得到。此处我们使用35号频点（即942MHz）（这里我们是通过频谱扫描确定的频点，建议你们也先观察一下，看哪个频点是可用的。）



```
#vim OpenBTS.config

# The initial global logging level: ERROR, WARN, NOTICE, INFO, DEBUG, DEEPDEBUG
Log.Level INFO
# Logging levels can also be defined for individual source files.
# This example set shows normal operations in the control layer (L3).
Log.Level.MobilityManagement.cpp INFO
$optional Log.Level.MobilityManagement.cpp
Log.Level.CallControl.cpp INFO
$optional Log.Level.CallControl.cpp
Log.Level.RadioResource.cpp INFO
$optional Log.Level.RadioResource.cpp

# The log file path.  If not set, logging goes to stdout.
Log.FileName openbts26.log
$static Log.FileName

# LOG(ALARM) is printed and also sent as udp to this address.
Log.Alarms.TargetIP 192.168.10.200
$optional Log.Alarms.TargetIP
Log.Alarms.TargetPort 10101
# Number of alarms saved in internal queue
Log.Alarms.Max 10

# Wireshark support
# OpenBTS writes L2 GSMTAP messages to this port.
# If GSMTAP.TargetIP is not defined, this output is disabled.
GSMTAP.TargetIP 127.0.0.1
$optional GSMTAP.TargetIP
# The standard IANA for GSMTAP is 4729.  This can be used to override that.
#GSMTAP.TargetPort 4729

# Indications port
# Some interal operations produce status reports sent to this port.
# This can be the same as the Alarm target.
Indications.TargetIP 127.0.0.1
Indications.TargetPort 10202


# Port number for test calls.
# This is where an external program can interact with a handset via UDP.
TestCall.Port 28670


#
# Transceiver parameters
#

# Transceiver interface
# This TRX.IP is not really adjustable.  Just leave it as 127.0.0.1.
TRX.IP 127.0.0.1
$static TRX.IP
# Path to transceiver binary
# If this is not defined, you will need to start the transceiver by hand.
# YOU MUST HAVE A MATCHING libusrp AS WELL!!
#TRX.Path ../Transceiver/transceiver
TRX.Path ../Transceiver52M/transceiver
$static TRX.Path

# TRX logging.
# Logging level.
# IF TRX.Path IS DEFINED, THIS MUST ALSO BE DEFINED.
TRX.LogLevel INFO
$static TRX.LogLevel
# Logging file.  If not defined, logs to stdout.
TRX.LogFileName TRX26.log
$static TRX.LogFileName



#
# SIP, RTP, servers
#

# Asterisk PBX
Asterisk.IP 127.0.0.1
Asterisk.Port 5060
$static Asterisk.IP
$static Asterisk.Port

# Messaging server
Smqueue.IP 127.0.0.1
Smqueue.Port 5063
$static Smqueue.IP
$static Smqueue.Port

# Local SIP/RTP ports
# OpenBTS binds to these ports.
SIP.Port 5062
$static SIP.Port
RTP.Start 16484
RTP.Range 98
$static RTP.Start
$static RTP.Range

# If Asterisk is 127.0.0.1, this is also 127.0.0.1.
# Otherwise, this should be the local IP address of the interface used to contact asterisk.
# In other words, this is the IP address at which Asterisk will see OpenBTS.
SIP.IP 127.0.0.1



#
# Special extensions.
#

# Routing extension for emergency calls.
# This must be defined, even if you are not supporting emergency calls.
PBX.Emergency 2101


#
# SIP parameters
#
#

# SIP registration period in seconds.
# Ideally, this should be slightly longer than GSM.T3212.
SIP.RegistrationPeriod 7200

#
# SIP Internal Timers.  All timer values are given in millseconds.
# These are from RFC-3261 Table A.
#

# SIP Timer A, the INVITE retry period, RFC-3261 Section 17.1.1.2
# in milliseconds
# For fast SMS interactions, keep this at 2000 or more.
SIP.Timer.A 2000



#
# SMS parameters
#
# ISDN address of source SMSC when we fake out a source SMSC.
SMS.FakeSrcSMSC 0000
# ISDN address of destination SMSC when a fake value is needed.
SMS.DefaultDestSMSC 0000


# The SMS HTTP gateway.
# Comment out if you don't have one or if you want to use smqueue.
#SMS.HTTP.Gateway api.clickatell.com

# IF SMS.HTTP.Gateway IS DEFINED, SMS.HTTP.AccessString MUST ALSO BE DEFINED.
#SMS.HTTP.AccessString sendmsg?user=xxxx&password=xxxx&api_id=xxxx




# Things to query during registration updates.
#Control.LUR.QueryIMEI
$optional Control.LUR.QueryIMEI
#Control.LUR.QueryClassmark
$optional Control.LUR.QueryClassmark

# Does everyone get a TMSI, registered or not?
Control.LUR.TMSIsAll
$optional Control.LUR.TMSIsAll

# TMSI table controls

# Maximum number of TMSIs to track
Control.TMSITable.MaxSize 10000

# Maximum allowed ages of a TMSI, in hours.
Control.TMSITable.MaxAge 72

# Want persistent TMSIs?
Control.TMSITable.SavePath TMSITable.txt
$optional Control.TMSISavePath

# Open Registration and Self-Provisioning
# If this is defined, the network accepts LUR from unprovisioned handsets.
# This is required to support SMS-based auto-provisioning.
# This is a boolean -- either defined or not.
Control.OpenRegistration
$optional Control.OpenRegistration


#
# "Welcome" messages sent during IMSI attach attempts.
# ANY WELCOME MESSAGE MUST BE LESS THAN 138 CHARACTERS.
# ANY DEFINED WELCOME MESSAGE MUST ALSO HAVE A DEFINED SHORT CODE.
# Comment out any message you don't want to use.
#

# The message sent upon full successful registration, all the way through the Asterisk server.
# THIS MESSAGE IS NORMALLY REQUIRED FOR COMPLIANCE WITH AGPLv3.
Control.NormalRegistrationWelcomeMessage Welcome to OpenBTS! AGPLv3 openbts.sf.net. Your IMSI is
Control.NormalRegistrationWelcomeShortCode 0000

# Then message sent to accpeted open registrations.
# IF OPEN REGISTRATION IS ENABLED, THIS MUST ALSO BE DEFINED.
# THIS MESSAGE IS NORMALLY REQUIRED FOR COMPLIANCE WITH AGPLv3.
Control.OpenRegistrationWelcomeMessage Welcome to OpenBTS! AGPLv3 openbts.sf.net. Your IMSI is
# If you use SMS auto-provisioning, use that short code as the return address here.
Control.OpenRegistrationWelcomeShortCode 101

# Then message send to failed registrations.
#Control.FailedRegistrationWelcomeMessage Your handset is not provisioned for this network.
$optional Control.FailedRegistrationWelcomeMessage
Control.FailedRegistrationWelcomeShortCode 1000
#
# GSM
#

# Network and cell identity.

# Network Color Code, 0-7
# Also set GSM.NCCsPermitted later in this file.
GSM.NCC 0
# Basesation Color Code, 0-7
GSM.BCC 2
# Mobile Country Code, 3 digits.
# MCC MUST BE 3 DIGITS.  Prefix with 0s if needed.
# Test code is 001.
GSM.MCC 310
# Mobile Network Code, 2 or 3 digits.
# Test code is 01.
GSM.MNC 07
# Location Area Code, 0-65535
GSM.LAC 1000
# Cell ID, 0-65535
GSM.CI 10

# Network "short name" to display on the handset.
# This is optional, but must be defined if you also want to
# send current time-of-day to the phine.
GSM.ShortName OpenBTS
$optional GSM.ShortName

# A boolean telling whether or not to show country initials with the name.
GSM.ShowCountry
$optional GMS.ShowCountry

# Network and cell identity.

# Network Color Code, 0-7
# Also set GSM.NCCsPermitted later in this file.
GSM.NCC 0
# Basesation Color Code, 0-7
GSM.BCC 2
# Mobile Country Code, 3 digits.
# MCC MUST BE 3 DIGITS.  Prefix with 0s if needed.
# Test code is 001.
GSM.MCC 310
# Mobile Network Code, 2 or 3 digits.
# Test code is 01.
GSM.MNC 07
# Location Area Code, 0-65535
GSM.LAC 1000
# Cell ID, 0-65535
GSM.CI 10

# Network "short name" to display on the handset.
# This is optional, but must be defined if you also want to
# send current time-of-day to the phine.
GSM.ShortName OpenBTS
$optional GSM.ShortName

# A boolean telling whether or not to show country initials with the name.
GSM.ShowCountry
$optional GMS.ShowCountry

# Assignment type for call setup.
# The default is early assignment.
# If defined, this will cause us to use very early assignment instead.
#GSM.VEA
$optional GSM.VEA

# Band and Frequency

# Valid band values are 850, 900, 1800, 1900.
GSM.Band 900
$static GSM.Band

# Valid ARFCN range depends on the band.
GSM.ARFCN 73
# ARCN 975 is inside the US ISM-900 band and also in the GSM900 band.
#GSM.ARFCN 975
# ARFCN 207 was what we ran at BM2008, I think, in the GSM850 band.
#GSM.ARFCN 207
$static GSM.ARFCN

# Neighbor list
# Should probably include our own ARFCN
GSM.Neighbors 39 41 43
#GSM.Neighbors 207


# Expected environmental delay spread, in symbols, at about 1.1 km/sym.
# FIXME -- Should be a TRX parameter.
GSM.MaxExpectedDelaySpread 1
# Receiver Gain, in dB
# comment out to set receiver to maximum possible receive gain
# With RxGain 57, -71.0dBm <-> 0dB RSSI, -113dBm <-> -38.2dB RSSI, -120dBm <-> -41.4dB RSSI
# With RxGain 47, -57.4dBm <-> 0dB RSSI, -113dBm <-> -51.3dB RSSI, -120dBm <-> -54.5dB RSSI
# With RxGain 37, -43.6dBm <-> 0dB RSSI, -113dBm <-> -63.1dB RSSI, -120dBm <-> -65.4dB RSSI
GSM.RxGain 47


# Downlink adaptive power management.
# Manages power to prevent congestion.

# Downlink tx power level bounds, dB wrt full power
# For the Mk 1B 900:
# Output power of USRP is 20 dBm at full scale.
# We installed a 10 dB attenuator.
# Gain of HPA-900 is 40 dB.
# So the output power, in dBm, is Po = 20 + 40 - 10 - Atten = 50 - Atten.
# So Atten = 50 - Po.
# That has been verified on the CMD57.
# HPA-900 max ouput is 42 dBm: Atten = 50 - 42 = 8 dB.
# Balanced link at 33 dBm: Atten = 50 - 33 = 17 dB.
# Maximum attenuation (sets MINIMUM power level).
# Atten 30 -> 20 dBm.
GSM.PowerManager.MaxAttenDB 30
# Minimum attenuation (sets MAXIMUM power level).
# Atten 7 -> 36 dBm. (with TX IF of 40MHz)
GSM.PowerManager.MinAttenDB  7

# Target T3122 for Power Manager.
# Under heavy load, the power manager will adjust downlink power
# so that T3122 meets this taret.
# THE TARGET T3122 MUST BE BETWEEN THE MIN AND MAX VALUES.
# THOSE ARE SET SOMEWHERE ELSE IN THIS FILE, GSM.T3122Max, GSM.T3122Min.
GSM.PowerManager.TargetT3122 5000
# T3122 sampling period, in milliseconds.
# PowerMananger.SamplePeriod should be <= PowerMananger.Period.
GSM.PowerManager.SamplePeriod 2000
# Sample history for averaging T3122, MUST BE <100;
GSM.PowerManager.NumSamples 10
# Power manager adaptation step period, in milliseconds
# Should be >= PowerMananger.SamplePeriod
GSM.PowerManager.Period 6000


# Channel configuration
# Number of C-VII slots (8xSDCCH)
GSM.NumC7s 1
$static GSM.NumC7s
# Number of C-I slots (1xTCH/F)
GSM.NumC1s 6
$static GSM.NumC1s
# Half-duplex option for low-capacity on cheap hardware.
#GSM.HalfDuplex
$optional GSM.HalfDuplex
#$static GSM.HalfDuplex


#
# GSM Timers.  All timer values are given in milliseconds unless stated otherwise.
# These come from GSM 04.08 11.2.
#

# T3212, registration timer.
# Unlike most timers, this is given in MINUTES.
# Actual period will be rounded down to a multiple of 6 minutes.
# Any value below 6 minutes disables periodic registration, which is probably a bad idea.
# Valid range is 6..1530.
# Ideally, this should be slightly less than the SIP.RegistrationPeriod.
GSM.T3212 12

# T3122, RACH holdoff timer.
# This value can vary internally between the min and max ends of the range.
# When congestion occurs, T3122 grows exponentially.
GSM.T3122Min 2000
# T3211Max MUST BE NO MORE THAN 255 s.
GSM.T3122Max 255000

# T3113, the paging timer
# This is the time allowed for a handset to respond to a paging request.
GSM.T3113 10000


# RRLP

# RRLP query conditions
# These booleans just have to be defined.  The actual values don't matter.
#GSM.RRLP.LUR
#$optional GSM.RRLP.LUR
#GSM.RRLP.MOC
#$optional GSM.RRLP.MOC
#GSM.RRLP.MTC
#$optional GSM.RRLP.MTC
#GSM.RRLP.MOSMS
#$optional GSM.RRLP.MOSMS
#GSM.RRLP.MTSMS
#$optional GSM.RRLP.MTSMS

# Accuracy requested from MS - 60 is about 400m (scale is approx. logarithmic)
GSM.RRLP.Accuracy 60
# Number of requests done per RRLPQueryController::doTransaction
GSM.RRLP.Retries 1
# RRLP query timeout, ms
GSM.RRLP.Timeout 4000

# The maximum AGCH queue length.
GSM.AGCH.QMax 5

# The uplink RSSI target for closed loop power control.
# For a "naked" USRP, this should be around -15 dB.
GSM.RSSITarget -15

# Number of channels reserved for paging responses.
# This is useful to insure paging response bandwidth when LUR loads are very high,
# but for most applications should just be zero.
GSM.PagingReservations 0

# Maximum internal FIFO latency, in vocoder frames.
# Trade-off is dropped frames vs. delay.
GSM.MaxSpeechLatency 2

#
# CLI paramters
#

CLI.Prompt OpenBTS>




```
