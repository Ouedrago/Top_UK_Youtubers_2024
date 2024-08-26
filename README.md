# Data Portfolio

## SQL CODE
```sql

/*

Data Cleaning steps

1. Remove unnecessary columns by selecting only the once we need
2. Extract the youtube channel names from the first column
3. Rename the columns

*/

SELECT 
     NOMBRE,
	 total_subscribers,
	 total_views,
	 total_videos

FROM 
     top_youtubers_2024



-- Step 1

DECLARE @conversionrate FLOAT = 0.02; -- coversion rate @ 2%
DECLARE @productCost MONEY = 5.0; -- Product cost @ 5 USD
DECLARE @campaignCost MONEY = 50000.0; -- Campaign cost @ 50000 usd

-- Step 2

WITH ChannelData AS (
     SELECT 
         channel_name,
	     total_views,
	     total_videos,
	     ROUND((CAST(total_views as FLOAT) / total_videos), -4) AS rounded_avg_views_per_video
		 --(total_views / total_videos) AS original_avg_views_per_video
     FROM
	      dbo.view_uk_youtubers_2024


)

SELECT 
     channel_name,
	 rounded_avg_views_per_video,
	 (rounded_avg_views_per_video * @conversionrate) AS potential_production_sale_per_video,
	 (rounded_avg_views_per_video * @conversionrate * @productCost) AS potential_revenue_per_video,
	 (rounded_avg_views_per_video * @conversionrate * @productCost)- @campaignCost AS net_profit
FROM 
     ChannelData
WHERE 
     channel_name IN ('NoCopyrightSounds','DanTDM','Dan Rhodes')
```
ORDER BY
     net_profit DESC


