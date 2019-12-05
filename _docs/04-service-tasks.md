---
title: "Service Tasks"
permalink: /docs/service-tasks
excerpt: "An example exploring how to develop service tasks."
last_modified_at: 2019-12-03T21:36:11-04:00
redirect_from:
  - /theme-setup/
toc: false
---

A _Service Task_ is a task that is executed (automatically) by the process engine when it became available. See the TravelPlan example below:

<img width="391" alt="Screen Shot 2019-09-30 at 11 38 02" src="https://user-images.githubusercontent.com/4369840/65893983-f3f3d480-e376-11e9-8107-3298e2e54ec8.png">


<img width="866" alt="Screen Shot 2019-09-30 at 10 19 43" src="https://user-images.githubusercontent.com/4369840/65890199-7e850580-e370-11e9-8660-89d5b35333a4.png">

We included three service tasks to calculate the travel plan final price. Which means these tasks will be executed automatically by the process engine.

We recommend the use of the _Delegate Strategy_ to implement the service tasks. See the example below:

<img width="955" alt="Screen Shot 2019-09-30 at 10 31 13" src="https://user-images.githubusercontent.com/4369840/65890232-88a70400-e370-11e9-938b-839390370e69.png">

The expression ``${travelPlanCalculateTotalPriceDelegate}`` is a reference to a Spring component that implements the interface ``JavaDelegate``.

See below the code of the ``travelPlanCalculateTotalPriceDelegate``component.

    package com.mycompany.myapp.camunda.delegate;


   	import com.mycompany.myapp.domain.TravelPlanDomain;
	import com.mycompany.myapp.repository.TravelPlanDomainRepository;
	import com.mycompany.myapp.service.dto.TravelPlanProcessDTO;
	import org.camunda.bpm.engine.delegate.DelegateExecution;
	import org.camunda.bpm.engine.delegate.JavaDelegate;
	import org.springframework.beans.factory.annotation.Autowired;
	import org.springframework.stereotype.Component;

	import java.math.BigDecimal;

    @Component
    public class TravelPlanCalculateTotalPriceDelegate implements JavaDelegate {

        @Autowired
        private TravelPlanDomainRepository travelPlanDomainRepository;

        @Override
        public void execute(DelegateExecution delegateExecution) throws Exception {
            TravelPlanProcessDTO travelPlanProcessDTO = (TravelPlanProcessDTO) delegateExecution.getVariable("pi");
            TravelPlanDomain travelPlanDomain = travelPlanDomainRepository.findOne(travelPlanProcessDTO.getTravelPlanDomain().getId());

            BigDecimal finalPrice = BigDecimal.ZERO;
            if (travelPlanDomain.getFlightPrice() != null) {
                finalPrice = finalPrice.add(travelPlanDomain.getFlightPrice());
            }
            if (travelPlanDomain.getHotelPrice() != null) {
                finalPrice = finalPrice.add(travelPlanDomain.getHotelPrice());
            }
            if (travelPlanDomain.getCarPrice() != null) {
                finalPrice = finalPrice.add(travelPlanDomain.getCarPrice());
            }
            travelPlanDomain.setFinalPrice(finalPrice);


            travelPlanDomainRepository.save(travelPlanDomain);
        }

    }


As a Spring component, you can inject other Spring components into it. Note also the variables ``pi`` and ``processInstance``refer to the process instance and were injected automatically by the application. You can also set new variables into the process engine context if you need it in later in the process execution (see the [Gateways](Gateways) documentation).