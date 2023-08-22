1.  Introduction

> This project is a result of my learning process as a Data/Business
> Analyst from the lat year also and the knowledge I have adquired in
> businesses during the last 7 years.
>
> The project is a real-case and aims to assist a new company who needs
> to understand the photovoltaic industry and identifying their
> strenghts and weaknesses. We must be able to operate and assess our
> performance in a timely and efficient manner. By analyzing our
> operations, we can identify areas where we excel and where we may need
> improvement, allowing us to adjust our strategies and investments
> accordingly. The ultimate goal is to provide actionable insights that
> will enable the company to make informed decisions and drive the
> business towards success.


<iframe width="800" height="600" src="<iframe title="Business Dashboard" width="1140" height="541.25" src=https://app.powerbi.com/reportEmbed?reportId=392aeb58-2460-4ae1-b418-f14771110c56&autoAuth=true&ctid=d05d4c80-da1e-4cd7-83a6-0d2094b20418 frameborder="0" allowFullScreen="true"></iframe>"></iframe>




2.  The Problem - Clarification Questions

> The Board of Direction (= company’s admistration) needs a solution
> that allows them to track their business and understand some insights.
> To accomplish this task, I must to build a business/operational
> dashboard that allows:

    1.  They want to know how is going the business (detail costs, revenue,
    profits, etc)
    2.  Be possible to analyze each project in detail.
    3.  Understand operativelly where we are competitive and where we are
    not.

> The start point of my journey: the company only have a a time_logging
> app (to register the workers time) and a simple accounting table.
>
> The previous step of my work was the creation and implemention of
> automated strategic data gathering procedures ir order to create a
> eficcient Data Model. However, due to their length, these procedures
> will not be detailed in this document.

3.  **Concepts & Methodology**

There are some concepts associated to the industry (i.e. construction)
and others related to conception of the model.

-   **Types of projects (3)**: Residential, Industrial and Utility
    Scale.

-   **Measure Revenue & % Ejecution:** since any project has a “Nº of
    Budget Hours” (means, the working hours that we must invest into the
    project), the Administration require the REVENUE was measure in a
    daily basis (“daily_revenue”), accordelly to the “Daily Worked
    Hours”. To understand this concept it is important to distinguish
    two (2) differente ways to measure the revenue:

    -   Residencial Projects: you measure in a way that if you surpass
        the “budget hours”, you are in lost, OR if the team finnish
        earlier, you are in profit (of course we must add the remain
        costs, such as the cost of travel, some invoice from material).

    -   Industrial and Utility Scale Projects: this type of projects can
        last weeks or even months, and to measure the revenue you must
        create a process based on tasks performed and the time it takes
        to complete each task, varying with the number of workers
        involved in the project (which may vary over the course of
        days). I like to think like a “Loading bar/Progress bar” like
        when you are charging a battery. For example, if a utility scale
        PV system has 100 “tables” (and each table has 60 PV modules),
        we are not mount each table from beggining to end, because we
        must to excel the mounting process. First we will mount the
        “pile driving” of all tables. Then we will assembly all the
        frames, there all the crossbeam, then all the straps, etc.
        Therefore, with this metodology we can measure the “% Theoric
        Progress” VS “% Actual Progress” and the “Theoric Daily Revenue
        (€)” VS “Actual Daily Revenue (€)” for each project (I don’t
        detail that spreadsheet here on this document).

-   **Travel Costs:** the company has some vehicles. The invoices from
    Gas and Renting from Leasing Company to those vehicles, are billing
    in a monthly basis. You can calculate easily the “DAily Renting
    Amount” for each vehicle, however, the issue is the “GAS” bought
    throught the Leasing Gas Card because does not detail which vehicle
    use it. The goal is to allow us to impute each cost to the certain
    project on a daily basis. Therefore you can’t impute the Leasing
    Card to a certain vehicle BUT you can make an estimation of the
    distance each vehicle had travel each day (using GPS trackers, for
    example) and the consumption of each vehicle. Due this reasons, the
    adminstration choose to install GPS trackers to track the vehicles
    distance route/cost and “eliminate” (from the “Operational
    Accounting”) the invoices from the Leasing Company. The cost of the
    vehicle is calculated as = cost renting/per day + cost of km’s. For
    that we have created a separeted sheet that imports the GPS distance
    and multiply for the gasoil price (always superior to allow us to
    have some margin) and the comsumption of each vehicle (roughly
    13L/100 km). For this reason we must “eliminate” from our Accounting
    sheet the costs of this particular supplier (Leasing Company).

-   **Track Specific Operational Costs:** to track all operational
    specific costs of a project, I used a “unique key” called
    “ProjectCode”. This way i can map all specific costs, such as the
    worker cost time, invoices, travels, etc. For example:

    -   Work entrance in the fLabor_force (time worked by each employee
        in a day) must have a ProjectCode (simply join/merge the
        Work_Schedule_Calendar with the fLabor_force using the key
        “emp_id”).

    -   Invoices in fAccounting table has to have a ProjectCode.

    -   Every “travel” (the way the workers move to the project
        location) must have a ProjectCode identifier.

-   **Track Non-Specific Operational Costs: w**hat happen if there is an
    invoice (like “tools”) that we cannot impute to a specific Project?
    In the cases related with operational costs that are not associated
    with any specific project, I choose to use the ProjectCode called
    “G001”. Them we must to calculate the “effort ratio” (=time)
    dedicated on each project on each month and imputed proportionally
    to each project. This way we can find the “real costs” of any
    project.

-   **Workers Time Cost (”Cost Worked Hours”)**

> It is important to calculate the net cost of each worker, i.e. the
> company cost. The cost per hour per worker is dynamic, and can vary
> every month. There are some details that are important to mention:

-   Every employee has a “job_category”, that means each employee has a
    anual salary, depending on the job category.

-   Each time a employee go to work, the employee receives 5€/day per
    assistance (according the Company-Union Agreement).

-   Some employees can receive “Team Bonus” for have the responsability
    of managing the team on site and report to the operations
    department. The value varies each month depending on some
    performance parameters.

-   Also some teams receive a “productivity bonus” based on the great
    work they have provided. This value varies monthly (a team leader
    can receive both bonus in the same month).

-   The salary is under the social security tax of 37%. That means that
    for paying 100€ to the employee, the company expense is 137,00€
    (excluded “assistance”).

-   The Company-Union Agreement establish that each worker SHOULD work
    not more than 1760h/year. This value is not mandatory but it is
    advisable to do not overloard the workers.

In resume, the way to calculate the total cost of a Project, is a
arithmetic sum of:

-   Workers Cost

-   Costs in general (= invoices, such as material expenses,
    accomodation, food), etc

-   Travels: the number of travels and the costs of gas
    (overestimation).

-   Proportional “G001” Costs.

1.  **Data Modeling**

Based on the methodology presented in the previous chapter, the Data
Model have the structure presented below. We must notice that there is
several “Fact Tables”, nonetheless, the relationships in the Data Model
are aiming to respect the “Star Schema” in order to maintaince the
reliability of the model.

1.  Dimensional Tables:

    1.  dCalendar (full calendar included the Company-Union Agreement
        holidays)

    2.  dLocation/Geography

    3.  dProjectCode

    4.  dEmployee

    5.  dVendor_ID

2.  Fact Tables:

    1.  fAccounting

    2.  fLabor_time (shows the time worked per day per employee. Also
        merged the “WorkSchedule Calendar” that shows where each
        employee was on that day = using the ProjectCode unique key)

    3.  fRevenue_daily

    4.  fSalaries (backoffice)

    5.  fTravels_daily

3.  **Solution**

(Power BI embed)

1.  Conclusions

    1.  Comparating the different types of projects, we can conclude
        some preliminar deductions:

        1.  Projects with several Megawatts are very competitive
            (because there are many companies building the site at the
            same time, competting) and the probability of uncertain
            events (such rainy days, logistical delays) are greater.

        2.  The most profitable projects were “industrial” (between
            50-150 kWp) and “Utility Scale” projects less than 500 kWP)

        3.  The worst project (greater loss) were also an industrial
            (many rainy days during the install delayed the ejecution
            the costs of travels were very high). The catch seems to be
            that the company must plan better the projects and give
            previlege to industrial projects that are “close” to the
            headquarters (same state).

        4.  About Residential projects, they have less revenue, however
            they are an important source of “constant revenue” and they
            can be done in just 1 day. The data shows that residential
            is a safe bet a long term.

        5.  We can see that the were a “break” in the project solds in
            the last months.

        6.  

-   It important refer that it is necessary more data (However the data
    has quality, the quantity is not so much.
