---
title: "Gateways"
permalink: /docs/gateways
excerpt: "An example exploring how to develop gateways."
last_modified_at: 2019-12-03T21:36:11-04:00
redirect_from:
  - /theme-setup/
toc: true
---

A Gateway is a decision point in the process. 

To illustrate the use of gateways, consider the following example:

<img width="727" alt="Screen Shot 2019-09-30 at 15 54 19" src="https://user-images.githubusercontent.com/4369840/65932849-423bbe80-e3dd-11e9-93fc-8d36c51ff90e.png">

The gateway decides whether the _Reserve a hotel_ task should be executed based on the following rule: if the interval between the start and end dates is longer than one day, a hotel reservation is needed. Otherwise (for quick trips) a hotel reservation is not required. 
 
There are three strategies to implement this gateway:
- Using sequence flow expressions
- Using a bean method
- Using a java delegate

## Using sequence flow expressions

The first strategy consists in using the sequence flow expression to compute whether the sequence should be executed. See the figure below: 

<img width="1072" alt="Screen Shot 2019-09-30 at 16 02 21" src="https://user-images.githubusercontent.com/4369840/65933192-93987d80-e3de-11e9-9fbf-f5dd78ef4b07.png">

In this example the expression ``${pi.travelPlanDomain.endDate > pi.travelPlanDomain.startDate}`` compares the dates (start and end) and find whether the interval is longer than one day.

> **The variables _processInstance_ and _pi_ are automatically set and represent the process instance**

This strategy seems simple but as the rules that guide the behavior of the gateway tend to be more complex than the presented in this example, we do not recommend the use of sequence flow expressions to implement the gateway decision. Instead of that, we recommend one of the following strategies. 

## Using bean methods

The second strategy consists in using a _Service Task_ just before a gateway to prepare the variables that will be used by the gateway. The figures below show how our example is implemented with this strategy.

<img width="913" alt="Screen Shot 2019-09-30 at 18 05 13" src="https://user-images.githubusercontent.com/4369840/65931740-f7b84300-e3d8-11e9-972b-527700523551.png">

The _"Verify if need a hotel"_ task was included just before the gateway.

<img width="726" alt="Screen Shot 2019-09-30 at 18 05 40" src="https://user-images.githubusercontent.com/4369840/65931751-fbe46080-e3d8-11e9-8cec-41f8481e82c4.png">

This Service Task invokes the method ``${travelPlanDomainService.verifyIfNeedAHotel(processInstance)}`` and put the result value into the variable ``needAHotel``.

<img width="481" alt="Screen Shot 2019-09-30 at 23 29 42" src="https://user-images.githubusercontent.com/4369840/65933775-de1af980-e3e0-11e9-9267-90f9373495c0.png">

The sequence flow expression is just the variable: ``${needAHotel}`` (much simpler than the expression in the first strategy).

The method ``verifyIfNeedAHotel``in the component ``travelPlanDomainService``is shown below:

    public Boolean verifyIfNeedAHotel(TravelPlanProcessDTO travelPlanProcessDTO) {
        long daysBetween = ChronoUnit.DAYS.between(travelPlanProcessDTO.getTravelPlanDomain().getStartDate(), travelPlanProcessDTO.getTravelPlanDomain().getEndDate());
        return daysBetween > 0;
    }

Since this strategy only allow to define one variable per service task, it works fine if the gateway uses only one variable. 
In cases where the gateway needs more than one variable, instead of using many service tasks, we suggest the next strategy.

## Using a java delegate

The main difference between the previous strategy is the use of a java delegate in the service task. See the images below:

<img width="639" alt="Screen Shot 2019-10-01 at 00 30 15" src="https://user-images.githubusercontent.com/4369840/65934284-aad96a00-e3e2-11e9-9147-2e3315616051.png">

The service task uses the delegate ``travelPlanNeedAHotelDelegate``. The code of this delegate is shown below:

	package com.mycompany.myapp.camunda.delegate;


	import com.mycompany.myapp.service.dto.TravelPlanProcessDTO;
	import org.camunda.bpm.engine.delegate.DelegateExecution;
	import org.camunda.bpm.engine.delegate.JavaDelegate;
	import org.springframework.stereotype.Component;

	import java.time.temporal.ChronoUnit;

	@Component
	public class TravelPlanNeedAHotelDelegate implements JavaDelegate {

	    @Override
	    public void execute(DelegateExecution delegateExecution) throws Exception {
	        TravelPlanProcessDTO travelPlanProcessDTO = (TravelPlanProcessDTO) delegateExecution.getVariable("pi");

	        long daysBetween = ChronoUnit.DAYS.between(travelPlanProcessDTO.getTravelPlanDomain().getStartDate(), travelPlanProcessDTO.getTravelPlanDomain().getEndDate());
	        Boolean needAHotel = daysBetween > 0;
	        
	        delegateExecution.setVariable("needAHotel", needAHotel);

	    }

	}

The delegate finds the difference between the two dates and set the variable ``needAHotel`` into the process execution context. The sequence flow expression is the same shown in the second strategy (``${needAHotel}``). Note that using a java delegate, you can set as many variables into the process execution as you need.

We just showed three strategies to implement gateways in our platform. We strongly recommend you to use one of the two last strategies.
