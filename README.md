---------------------------------------------------------------------------[[↓setup↓]]
chance      = 20.0000            -- chance to play
iol         = 1.25000            -- action on loss in muliplier "x"
dow         = 1.0                -- action on win in muliplier "x"
roundprofit = 0.00000001         -- round profit to reset the cycle
siteminbet  = 0.00000001         -- sites minimumbet 
divider     = 527034             -- divide your balance / x for basebet
bethigh     = true               -- bet over(true) or under(false)
won         = 0
lose        = 0
totalbets   = 0

seedcount = 10000                -- resets seed every x bets
---------------------------------------------------------------------------[[↑setup↑]]

basebet = 0.00005000;

nextbet = basebet;initialbalance = balance;wager,wagerpercbal,largebalance,loseamount,largestloss,prof=0,0,0,0,0,0;resetseed();resetstats()

playtime = os.clock()

function reset()
  nextbet = basebet
  prof    = 0
end

function dobet()
  totalbets+=1
    if (win) then
        nextbet = basebet
        won     = won+1
        lose    = 0
    else
        nextbet = (previousbet * iol)
        lose    = lose+1
        won     = 0
    end



  if won==10 then
  bethigh=!bethigh
  won=0
  end
  if lose==5 then
  bethigh=!bethigh
  lose=0
  end


  if (totalbets % seedcount == 0) then
    resetseed()
    totalbets = 0
  end



losspercent      = largestloss / initialbalance * 100
profitpercentage = profit / (balance - profit) * 100
wager            = (wager + nextbet)
wagerpercbal     = (wager / initialbalance)
prof             = (prof + currentprofit)

if prof         >= roundprofit then reset() end
if nextbet      <= siteminbet then nextbet = siteminbet end
if balance      >= largebalance then largebalance = balance loseamount = 0 else loseamount = (largebalance - balance) end
if loseamount   >= largestloss then largestloss = loseamount end

i=1;for i=1,25 do print("\n")print("")end
print("\n\nx>>>>>>>>>>>>>>>><<<<<<<<<<<<<<<<<x\n\n")
print("payout: " .. string.format("%.2f", (99 / chance)) .. "x" .. " @ " .. string.format("%.2f", chance) .. "% chance")
print("profit: " .. string.format("%.8f", profit) .. " | " .. string.format("%.4f", profitpercentage) .. "%")
print("largest loss: " .. string.format("%.8f", largestloss) .. " | " .. string.format("%.4f", losspercent) .. "%")
print("wager: " .. string.format("%.8f", wager) .. " | " .. string.format("%2.4f", wagerpercbal) .. "x")
print("~bets per: " .. string.format("%2.3f", bets/(os.clock()-playtime)) .. " /second | " .. math.floor(0.5+bets/(os.clock()-playtime)*60^2) .. " /hour | " .. math.floor(0.5+24*bets/(os.clock()-playtime)*60^2) .. " /day")

print("\n\nx>>>>>>>>>>>>>>>><<<<<<<<<<<<<<<<<x\n\n") end
