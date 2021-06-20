### Fork

This project is cloned from [at](https://github.com/xlab/at)

### Installation

```
go get github.com/lx32056127/pdusms
```

### Features

This framework includes facilities for device monitoring, sending and receiving AT-commands, encoding and decoding SMS
messages from or to PDU octet representation (as specified in [3GPP TS 23.040]). An example of incoming SMS monitor
application is given in [example/daemon].

[example/daemon]: https://github.com/xlab/at/blob/master/example/daemon

[3GPP TS 23.040]: http://www.etsi.org/deliver/etsi_ts/123000_123099/123040/11.05.00_60/ts_123040v110500p.pdf

### Examples

To get an SMS in a PDU octet representation:

```go
smsSubmitGsm7 := Message{
Text:                 "hello world",
Encoding:             Encodings.Gsm7Bit,
Type:                 MessageTypes.Submit,
Address:              "+79261234567",
ServiceCenterAddress: "+79262000331",
VP:                   ValidityPeriod(time.Hour * 24 * 4),
VPFormat:             ValidityPeriodFormats.Relative,
}
n, octets, err := smsSubmitGsm7.PDU()
```

To open a modem device:

```go
dev = &Device{
CommandPort: CommandPortPath,
NotifyPort:  NotifyPortPath,
}
if err = dev.Open(); err != nil {
return
}
```

If you're going to use this framework and its methods instead of plain R/W you should initialize the modem beforehand:

```
if err = dev.Init(DeviceE173()); err != nil {
	return
}
```

To use the wrapped version of a command:

```go
err = dev.Commands.CUSD(UssdResultReporting.Enable, pdu.Encode7Bit(`*100#`), Encodings.Gsm7Bit)
```

Or to send a completely generic command:

```go
str, err := dev.Send(`AT+GMM`)
log.Println(str, err)
```

### Device-specific config

In order to introduce your own logic (i.e. custom modem Init function), you should derive your profile from the default
DeviceProfile and override its methods.

### License

[MIT](http://xlab.mit-license.org)
