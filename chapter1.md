# Registro Redhat

Para poder operar el SO Red Hat Enterprise Linux 7.5 es necesario tener una suscripción activa. 

### Prueba Gratuita 30 días

Ingresar al sitio [https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux?intcmp=70160000000HCbd ](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux?intcmp=70160000000HCbd) y obtener una prueba. 

### Dar de alta la suscripción activa

Para dar de alta la suscripción es necesario ingresar a la terminal del SO:

> sudo subscription-manager register

```
Registering to: subscription.rhn.redhat.com:443/subscription
Username: 
Password:
The system has been registered with ID: e7426eca-1764-4967-81fa-3031ef994b2e
```

Es necesario identificar el pool ID y darlo de alta

> subscription-manager list --available --all

```
You are attempting to run "subscription-manager" which requires administrative
privileges, but more information is needed in order to do so.
Authenticating as "root"
Password:
+-------------------------------------------+
    Available Subscriptions
+-------------------------------------------+
Subscription Name:   30 Day Red Hat Enterprise Linux Server Self-Supported Evaluation
Provides:            Red Hat Container Images
                     Red Hat Ansible Engine
                     Oracle Java (for RHEL Server)
                     Red Hat Enterprise Linux Atomic Host Beta
                     Red Hat Enterprise Linux Server
                     Red Hat Enterprise Linux Atomic Host
                     Red Hat Container Images Beta
                     Red Hat Beta
SKU:                 RH00065
Contract:            11683040
Pool ID:             8a85f98b635f3e6d0163a8ac83be69e8 <---- POOL ID
Provides Management: No
Available:           1
Suggested:           0
Service Level:       Self-Support
Service Type:        L1-L3
Subscription Type:   Instance Based
Ends:                06/26/2018
System Type:         Physical
```

> sudo subscription-manager attach --pool=8a85f98b635f3e6d0163a8ac83be69e8

### Una vez realizado esto será posible operar el SO Red Hat.



