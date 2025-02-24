# EMS - Introduction
---

## Embedded Systems (ES) vs. General Purpose (GP) Systems
| **Aspect**       | **General Purpose**          | **Embedded Systems**               |
|-------------------|-------------------------------|-------------------------------------|
| Purpose           | Run varied programs           | Optimized for specific tasks       |
| Constraints       | Complexity/cost secondary     | Strict timing, power, cost, reliability |
| Interaction       | User-centric (keyboard, GUI)  | Physical world (sensors/actuators) |
| OS Support        | Feature-rich (Linux, Windows) | Minimal or real-time OS            |
| Development Time  | Longer cycles                 | Tight deadlines (e.g., 6-month window) |

---

## ES Application Domains
1. **Consumer Electronics**:
   - Trillion-dollar market, short lifespan, low-power demands.
2. **Transportation**:
   - Safety-critical, certified tools, long development cycles.
3. **Industrial Systems**:
   - High reliability, tied to critical infrastructure.
4. **Biomedical Devices**:
   - Long lifespan, reliability-focused.

---

## Key ES Characteristics
- **Sophisticated Functionality**: Complex algorithms, multi-rate operations (e.g., audio + sensor data).
- **Real-Time Operation**:
  - **Hard**: Deadlines critical (e.g., CT scan).
  - **Soft**: Deadlines flexible (e.g., UI updates).
- **Cost & Power Efficiency**:
  - Mass-market focus, battery optimization.
- **Design Challenges**:
  - Limited observability, restricted dev environments.

---

## Choosing the Right ES Platform
- **Key Questions**:
  - Hardware needs (memory, peripherals)?
  - Meet deadlines via hardware or software?
  - Minimize power (e.g., logic shutdowns)?
  - Upgradeability and testing strategies?
- **Platform Options**:
  - **ASIC**: High design cost, low unit cost, fast.
  - **FPGA**: Reconfigurable, medium power/speed.
  - **SoC/MCU**: Balanced cost/power, easy upgrades.
  - **Embedded PC**: High flexibility, higher cost.

---

## Trade-Offs Table
| **Platform** | Design Cost | Unit Cost | Power Efficiency | Speed          |
| ------------ | ----------- | --------- | ---------------- | -------------- |
| ASIC         | Very High   | Very Low  | Low              | Extremely Fast |
| FPGA         | Low-Mid     | Mid       | Medium-High      | Very Fast      |
| SoC/MCU      | Low-Mid     | Mid-Low   | Medium           | Moderate       |
| Embedded PC  | Low         | High      | Medium-High      | Fast           |

---

## Historical Evolution
- **Computer Integration**:
  - Mainframes → PCs → mm-scale "motes" (sensors with CPU/memory).
- **Wireless Communication**:
  - Smaller transceivers, higher data rates (Wi-Fi, LTE).
- **Energy Harvesting**:
  - Piezoelectric, thermoelectric, RF, supercapacitors.

---

## Design Philosophies
- **Performance vs. Cost**: Faster hardware increases cost/power.
- **Energy Storage**: Trade-offs between battery life and heat.
- **Market Timing**: Delayed entry reduces peak revenue.
