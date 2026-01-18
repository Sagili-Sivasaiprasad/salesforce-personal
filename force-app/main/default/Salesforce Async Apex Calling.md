Salesforce Async Apex – Calling Matrix

📘 Table 1: Calling FUTURE
Called From	YES	NO
From Future	20	80
From Batch (Execute)	41	59
From Batch (Finish)	65	35
From Schedule	71	29
From Queueable	76	24

📘 Table 1: Calling FUTURE
Called From ↓	        Can we Call Future?
Future	                ❌ NO
Batch (Execute)	        ❌ NO
Batch (Finish)	        ✅ YES
Schedulable	            ❌ NO
Queueable	            ❌ NO

📘 Table 2: Calling BATCH
Called From  ↓	        Can We Call Batch?
Future	                ❌ NO
Batch (Execute)	        ❌ NO
Batch (Finish)	        ❌ NO
Schedulable	            ✅ YES
Queueable	            ❌ NO

📘 Table 3: Calling SCHEDULABLE
Called From  ↓	        Can Call Schedulable?
Future	                ❌ NO
Batch (Execute)	        ❌ NO
Batch (Finish)	        ❌ NO
Schedulable	            ❌ NO
Queueable	            ❌ NO

📘 Table 4: Calling QUEUEABLE
Called From ↓	        Can Call Queueable?
Future	                ❌ NO
Batch (Execute)	        ✅ YES
Batch (Finish)	        ✅ YES
Schedulable	            ✅ YES
Queueable	            ✅ YES (chaining)