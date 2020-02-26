How use Quartz Scheduler
========================

1. Quartz.Net.


   for use Quartz need:
   - install Nuget Quartz packet
   - define trigger whitch run scheduler
   - define operations whitch executed after trigger fire
   - define scheduler

   simple code example for console project 
.. code:: javascript

            IScheduler scheduler = StdSchedulerFactory.GetDefaultScheduler().Result;
            IJobDetail job = JobBuilder.Create<DoSomething>()
                .WithIdentity("DoSomethingJob","group1")
                .Build();
            ITrigger trigger = TriggerBuilder.Create()
                .WithIdentity("DOSomethingTrigger", "trigger_groups1")
                .StartNow()
                .WithSimpleSchedule(p =>
                    p.WithIntervalInSeconds(10)
                    .RepeatForever())                  
                .Build();
            scheduler.ScheduleJob(job, trigger);
            scheduler.Start();
   

Code example for dotnet.core project

.. code:: javascript

            services.AddSingleton<IJob, SomthingDo>();
            services.AddSingleton(provider => 
                JobBuilder.Create<SomthingDo>()
                    .WithIdentity("SomethingDo","CustomGroup")
                    .Build());
            services.AddSingleton(provider =>
                TriggerBuilder.Create()
                    .WithIdentity("SomethingDoTrigger", "CustomTriggerGroup")
                    .WithDailyTimeIntervalSchedule(p => p.WithIntervalInSeconds(10))
                    .Build());
            services.AddSingleton(provider =>
                {
                    var schedulerFactory = new StdSchedulerFactory();
                    var scheduler = schedulerFactory.GetScheduler().Result;
                    scheduler.ScheduleJob(provider.GetService<IJobDetail>(), provider.GetService<ITrigger>());
                    scheduler.Start();
                    return scheduler;
                });

2. Used Classes and Interfaces

   - IScheduler - main scheduler interface.
   - IJob - job interface that describes the action classes.
   - IJobDetails - describe action
   - ITrigger - trigger interface
   - JobBuilder - used for create job object
   - TriggerBuilder - used for create trigger object


3. JobDataMap

When creating the scheduler, possible to pass parameters to the task.

.. code:: javascript

            services.AddSingleton(provider => 
                JobBuilder.Create<SomthingDo>()
                    .WithIdentity("SomethingDo","CustomGroup")
                    .UsingJobData("pi", 3.14f)
                    .UsingJobData("g", 9.81f)
                    .Build());


For get input parameter in job classes necessary use JobDataMap

.. code:: javascript

            JobDataMap dataMap = context.JobDetail.JobDataMap;
            var pi = dataMap.GetDouble("pi");
            System.Console.WriteLine($"Pi: {pi}");
            var g = dataMap.GetFloat("g");
            System.Console.WriteLine($"g: {g}");

4. Triggers

   - possible to set the start time and end time of the trigger
   - possible to set trgger priority. Forexample, if at one moment in time 2 triggers fire at the same time, then  they wil be executed according to priority
   - possible exclude some date with Calendar

Forexample, triggers with different time settings

.. code:: javascript

            services.AddSingleton(provider =>
                TriggerBuilder.Create()
                    .WithIdentity("SomethingDoTrigger", "CustomTriggerGroup")
                    .StartNow()
                    // WithSimpleSchedule example
                    //.WithSimpleSchedule(p => p.WithIntervalInSeconds(10).WithRepeatCount(2))
                    //.StartAt(new System.DateTimeOffset(new System.DateTime(2020, 02, 25, 22, 00, 00)))
                    //.EndAt(new System.DateTimeOffset(new System.DateTime(2020, 02, 25, 23, 00, 00)))

                    // Cron example
                    //.WithCronSchedule("10 * * * * ?")
                    
                    // WithCalendarIntervalSchedule example
                    //.WithCalendarIntervalSchedule(p => p.WithIntervalInSeconds(10))
                    //.ModifiedByCalendar("myHoliday")
                    
                    // WithDailyTimeIntervalSchedule example
                    .WithDailyTimeIntervalSchedule(p => p.WithIntervalInSeconds(10))
                    .Build());


5. Listeners

There are 3 types of Listeners:

   - SchedulerListener - triggers events related with scheduler work
   - TriggerListener - triggers events related with Trigger work
   - Joblistener - triggers events related with Job work

They are created by implementing interfaces (ISchedulerListener, ITriggerListener, IJobListener). These interfaces describe defferent events, the implementation of each can be described.
Scheduler connect Listeners example:

.. code:: javascript
            
            services.AddSingleton(provider =>
                {
                    var schedulerFactory = new StdSchedulerFactory();
                    var scheduler = schedulerFactory.GetScheduler().Result;
                    scheduler.ScheduleJob(provider.GetService<IJobDetail>(), provider.GetService<ITrigger>());
                    scheduler.ListenerManager.AddSchedulerListener(new SchedulerListener());
                    scheduler.ListenerManager.AddTriggerListener(new TriggerListener(), GroupMatcher<TriggerKey>.AnyGroup());
                    scheduler.ListenerManager.AddJobListener(new JobListener(), GroupMatcher<JobKey>.AnyGroup());

                    scheduler.Start();
                    return scheduler;
                });


For each listener need create implementation file.

.. code:: javascript

               public class JobListener : IJobListener
               {
                  //interface implementation
               }


.. code:: javascript

               public class SchedulerListener : ISchedulerListener
               {
                  //interface implementation
               }


.. code:: javascript

               public class TriggerListener : ITriggerListener
               {
                  //interface implementation
               }
