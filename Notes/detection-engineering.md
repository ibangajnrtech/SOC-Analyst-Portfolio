# What is Detection Engineering?

Detection engineering is the continuous process of building and operating threat intelligence analytics to identify potentially malicious activity or misconfigurations that may affect your environment. 

## Detection Types

Threat detection can be viewed from two perspectives, each comprising two categories: 
- **Environment-based detection** which focuses on looking at changes in an environment based on configurations and baseline activities that have been defined. Within this detection, we have Configuration detection and Modelling.
- **Threat-based detection**: focused on elements associated with an adversary's activity, such as tactics, tools and artefacts that would identify their actions. We have indicators and Threat Behaviour.

### Configuration Detection

We use current knowledge of the known environment and infrastructure to identify misalignments. Configurations can cross domains, including network, asset or identity.

Benefits of Configuration detection:
1. The easiest form of detection to create and maintain in static environments.
2. Under perfect conditions and coverage, it detects all malicious activity.
3. Individuals with different expertise can execute the detection.
4. Easy to combine with other detections for forensics and response.

Challenges of Configuration detection:
1. Difficult to maintain in dynamic environments
2. Limited visibility reduces effectiveness.
3. There's an assumption of knowledge of the working infrastructure and configurations for effectiveness.
4. Frequent configurations can result in high false positives.

### Modelling

Threat detection under this type is done by defining baseline operations and activities and recording any deviations that occur. The primary assumption of this approach is that malicious activity can be sufficiently identified from benign activity.
The approach involves building an asset or activity profile that includes events, time and data threshold. 

Benefits of Modelling:
1. Used to identify unknown adversary activities due to model changes and not threat characteristics.
2. Easy to maintain in very static environments.

Challenges of Modelling:
1. Provides no context of threat activity during investigations.
2. Difficult to maintain in dynamic environments.
3. Limited visibility reduces effectiveness.
4. Assumes in-depth knowledge of the working infrastructure and configurations.
5. Potentially adds existing malicious activity into the model.

