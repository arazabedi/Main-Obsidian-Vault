- Write for humans first, compilers second.
- Clear, descriptive naming
- Modular functions that do one thing
- Avoid cleverness in favour of intuitiveness
- Lean on SOLID principles - particularly Single Responsibility and Dependency Inversion

- Dependency Inversion:
	- High-level modules (e.g. Car) don't depend on low-level modules (e.g. Vehicle), both depend on abstractions
	- Abstractions should not depend on details, details should depend on abstractions
- For example, a violation of DIP is a low level class of LightBulb being used in the high-level module of ElectricPowerSwitch. This is bad. If you want to switch something else like a Fan class, you'd need to modify the switch class.
- Instead, make an interface Switchable with methods `turnOff()` and `turnOn()` and implement the interface in LightBulb (and Fan), then use a Switchable type variable as a dependency of ElectricPowerSwitch.
- This loosely couples switch to the LightBulb/Fan through the Switchable interface, rather than concretely, making the Switch class more extensible and testable with a mock implementation.

- Consistent formatting and style conventions using linters/tools like Prettier, eliminating debates about style in code review and keeps focus on substance.
- Write comprehensive tests where possible including unit, integration, and end-to-end tests to serve as living documentation and catch bugs.
- Leave the campsite cleaner than I found it.

- All this helps keep the code readable, testable, and easy for others to work with.