---
title: CAF Implementation on FPGA Using Python Tools
description: The purpose of this project is to provide a real time geolocation
  solution by generating code for the complex ambiguity function (CAF) in a
  hardware description language (HDL) and the implementation on FPGA hardware.
abstract: The purpose of this project is to provide a real time geolocation
  solution by generating code for the complex ambiguity function (CAF) in a
  hardware description language (HDL) and the implementation on FPGA hardware.
  The CAF has many practical applications, the more traditional being radar or
  sonar type systems. By using scientific Python tools, this project provides a
  solution for testing signals and the ability to customize modules to target
  multiple devices. The processing for this implementation will be done on a
  PYNQ board designed by Xilinx. The PYNQ board provides a Zynq chip which has
  both an ARM CPU and FPGA fabric. All required mathematical operations for the
  CAF are returned to the user through Python classes which produce
  synthesizable code in the Verilog HDL. The Python classes use Jinja templates
  integrated into the Verilog code to allow for configuration changes that a
  user will need to change for investigation and simulation, development, and
  test. Helper methods are included in the package to help simulation of the HDL
  such as quantization, complex data reading and writing, and methods to verify
  the data using quantized values.
---

