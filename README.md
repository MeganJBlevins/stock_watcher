# foreup_test
  Thank you so much for letting me prove my skills and knowledge through this code test! I truly appreciate the opportunity!
  I'd love to go over my work with you if you hve any questions or comments about my process, decisions or how I did something. I am always up to learn better ways to do things!

  I look forward to hearing your thoughts!

  Megan Blevins
  MeganBlevins.com
  MeganJBlevins@gmail.com
  417.718.4470

## To run the project locally, in the project directory: 
```
yarn install
yarn run serve
```

## Notes from the developer:
  You'll notice the dropdown at the top of the project. I ran into some issues witht he api (limit to 5 calls per minute) that wouldn't be an issue on a production site, but were frustrating during testing. I decided to leave that in the project in case that helps with some QA also. 

  I validated the stock symbols using a third party's list, as I wanted to validate a stock symbol prior to the api call. It had the added benefit of having the company name also. The api didn't send that information in the daily call, so I would have had to do another call, and that would be frustrating. 
    - I only pulled the NASDAQ and NYSE symbols. So any others will not be found, but this could be expanded if needed.

  Apparently the alphavantage guys removed the batch api endpoint, so I had to loop through the initial stocks one by one, which isn't great... if there's not another option through alphavantage...

  If it's easier for you, I can host this through netlify for you to view without having to run it locally, just let me know! Thank you!

### Things I would tweak for a client:
  - I would add a way to remove stocks once added. (design needed)
  - I would add a cookie to cache the user's stock selections.
  - I would do more error handling in case the api fails. 
    
# questions for Design: 
  (I would usually request an XD or Sketch file)
  - What are the font sizes for the project? 
  - error handling styles for the input?
  - error verbiage for input if wanting custom?
  - How do you remove a stock from your list? what does that look like?
  - what does it look like when there are no stocks selected? 
  - what are the default stocks (pre-adding stocks) - if any.
  - how often should this refresh? (IDK how often stock stuff is updated anyway, this might not be a relevant question if the data remains the same every day)
  - what color is the background (body)
  - hover, active, focus states? 

