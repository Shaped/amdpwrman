  amdpwrman 0.01a        (C) 2020 Jai B. (Shaped Technologies)        MIT License

  amdpwrman shows statistics and manipulates power limit settings for AMD GPUs
  on Linux through the sysfs interface provided by the amdgpu driver. This script
  was designed to be very simple to use from the shell, requiring no external
  configuration.
  
  All values are reported by the sysfs interface provided by the amdgpu driver,
  sometimes these values may not be perfect, for example on an RX570, the max
  RPM of the fan is reported as 5300 RPM but at 100% fan speed the fan only hits
  3300 RPM. I've also noticed this behavior with apps on Windows even, so it is
  not isolated to Linux, nor is it an issue with this script.
  
  Note: amdpwrman will require root access to modify values.
  Attempting to recover a GPU may require the amdgpu.gpu_recovery=1 kernel flag.

  This script was created because I needed a simple way to manage my AMD mining GPUs
  from the console.
  
  Usage:

      If <gpu> is ommitted from any command, GPU0 is used. Note that GPU0 only
      refers to the first AMD GPU reported by the amdgpu driver.

      amdpwrman help | -h            Display this help message.
      amdpwrman show <gpu>           Shows detailed statistics for <gpu>
      amdpwrman power <limit> <gpu>  Sets power limit to <limit> in watts for <gpu>
      amdpwrman power reset <gpu>    Resets power limit for <gpu>
      amdpwrman fan enable <gpu>     Enables manual fan speed control for <gpu>
      amdpwrman fan disable <gpu>    Disables manual fan speed control for <gpu>
      amdpwrman fan <speed> <gpu>    Sets fan speed for <gpu>
      amdpwrman recover <gpu>        Attempt to recover a halted or crashed <gpu>

      GPU0..9 refers to the 0..9th AMD GPUs reported by the amdgpu driver.

   NOTE: GPU index numbers are in order reported by the amdgpu driver, so GPU0
   will always be the first AMD GPU regardless of what other vendor's adapters
   are present in the system.

  Examples:

      #$ ./amdpwrman show		Show stats for GPU0 (TODO: show stats for all gpus)
      #$ ./amdpwrman power 100		Sets power limit to 100W for GPU0
      #$ ./amdpwrman power 100 3	Sets power limit to 100W for GPU3
      #$ ./amdpwrman fan enable 3	Enables manual fan control of GPU3
      #$ ./amdpwrman fan 100 3		Sets GPU3's fan speed to 100%
      #$ ./amdpwrman fan disable	Disables manual fan control of GPU0
      #$ ./amdpwrman power reset	Resets the power limit of GPU0

  FAQ:

	1q. Why?
	1a. Why not? I needed an easy way to do this and got sick of manipulating the files by hand, so I whipped up this bash script in a few hours.

	2q. Why AMD?
	2a. While historically I have generally prefered NVidia cards, I do use AMD cards for mining currently as they are more economical (An RX570 provides about the same hashrate as a RTX2060 for Etherium for example).

	3q. Will you support NVidia?
	3a. I'd like to, I haven't looked into what interfaces are available for the NVidia drivers. I currently don't have any suitable NVidia cards in any Linux boxen to test with. Maybe one day.

	4q. Can I contribute?
	4a. If you have additions that may be useful or find any bugs, feel free to send them in.

	5q. MIT License?
	5a. Yep. This is a quick hacky shell script that I spent more time on than I planned and figured it might be worth sharing. Do whatever the fack you want with it, just maybe at least refer to me as the original author though.

  TODO:

      - support multiple GPUs at once, eg. 'amdpwrman show all'.
      - change 'show' without a GPU specifier to show all GPUs once multigpu support is complete.
      - fan curve control has started to be implemented
      - fan curve control currently only selects the next highest fan setting and uses that
      - fan curve control needs to interopolate fan speeds between settings instead of simply going to the next highest
      - fan curve control needs to be tweaked to support multiple GPUs at once - while it's been structured for multilpe GPUs from the get-go, it currently only will allow one instance to run. simply allowing multiple instances would allow fan control of multiple GPUs but I feel a single instance for all GPUs is best
      - fan curve control format is: delay temp:fanspeed ... temp:fanspeed gpu .. if we are managing all GPUs in one instance, then we need to either have the same delay for all GPUs, which means another way of setting it instead of it simply being the first variable in the list OR we need to actually do some more fun stuff so that it updates each GPU based off it's waittime
      - fan curve control should have options for linear and log scaling/interop
