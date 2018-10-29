local cpt = require("component")
local io = require("io")
local term = require("term")
local event = require("event")
--local color = require("colors")
local gpu = cpt.gpu
local fileName = "/home/gtadrs15.txt"
local fileWrite
local fileRead
local crash

local color = { }
color["red"]   = 0xFF0000
color["green"] = 0x00FF00

  
gpu.setForeground(0x00F000)
--fromgtadrs15 = true
if not cpt.isAvailable("draconic_reactor") then 
print ("подключите адаптер к стабилизатору реактора * attach adapter to draconic reactor")
term.read()
return
end
local react = cpt.draconic_reactor

printf = function (s,...)
return io.write(s:format(...))
end
 function colorprintf(c,s,...)
 local oldcolor = gpu.getForeground()
  gpu.setForeground(c)
  printf (s,...)
  gpu.setForeground(oldcolor)
  end
fileWrite=io.open(fileName,"a")
fileWrite:close()

if upadr ~= nil or downadr ~= nil then

if cpt.get(downadr) == nil or cpt.get(upadr)==nil then
print ("упс, кто-то сломал ваши адаптеры и адреса изменились *oops, someone crashed your adapters and changed addresses")
upadr=nil
downadr = nil
crash = true
term.read()
return
end
else fromgtadrs15 = true
end
if ( fromgtadrs15 == nil or fromgtadrs15 == true) and (crash==false or crash == nil) then
fileRead = io.open(fileName,"r")
upadr,downadr =fileRead:read("*l","*l")
fileRead:close()
end


while upadr == nil  or downadr == nil do 
local i = 0
for address in cpt.list("flux_gate") do
i=i+1
end
if i < 2  then 
print ("подключите адаптеры к флюкс-гейтам *attach adapters to flux-gates")
term.read()
return

end
print ("адреса подключенных флюкс-гейтов *attached flux_gate addresses\n")

for address in cpt.list("flux_gate") do
print (address,"\n")
end

while upadr == nil do
print ('введите первые символы адресса гейта, отвечающего за поток *input "flow gate" address first dijits')
upadr = cpt.get(io.read(),"flux_gate")
if upadr == nil then print ("Неверный адресс *incorrect address\n")

end
end
while downadr == nil or downadr == upadr do
print ("Введите первые символы адреса гейта отвечающего за сдерживание *input shield gate address first dijits")
downadr = cpt.get(io.read(),"flux_gate")
if downadr == nil or downadr == upadr then print ("Неверный адресс *incorrect address\n")
end
end
local conf = true
while conf do

printf ("\nАдресс поточного гейта      *Flow gate adress:     %s \n",upadr)
printf ("Адресс сдерживающего гейта  *Shield gate adress:   %s \n",downadr)
print ("\nвсе верно *All correct?\n\n1-Да, продолжить *Yes. Continue\n2-Нет, изменить *No. Change")
local chk = io.read()
if chk  == "1" then conf = false  
elseif chk == "2" then 
upadr = nil
downadr = nil
term.clear()
conf = false
else 
print ("\nне понял (введите 1 или 2) * i don't understald (type 1 or 2)\n")

end

end
 
crash = false
fromgtadrs15 = true 
end
fileWrite=io.open(fileName,"w")
fileWrite:write(upadr.."\n"..downadr.."\n")
fileWrite:close()

--cpt.setPrimary("flux_gate",upadr)
local upgate = cpt.proxy(cpt.get(tostring(upadr)))
local downgate = cpt.proxy(cpt.get(tostring(downadr)))


function keydown(eventname,keyboardaddress,char,code,playername)
end

term.clear()

colorprintf(color["red"],"press enter to start")
term.read()

prog = true
dynamic = false 



while(prog==true) do

if cpt.get(upadr) ~= nil and cpt.get(downadr) ~= nil and cpt.isAvailable("draconic_reactor") then
tst1 = react.getReactorInfo()
if cpt.isAvailable("draconic_rf_storage") then
local core = cpt.draconic_rf_storage


local coreT = core.getEnergyStored()/10^12
local coreB = core.getEnergyStored()/10^9
local coreM = core.getEnergyStored()/10^6
local coreK = core.getEnergyStored()/10^3
if core.getEnergyStored() > 10^12 then
printf ("накоплено в ядре: %0.3f T\n\n",coreT)
elseif core.getEnergyStored() > 10^9 then 
printf ("накоплено в ядре: %0.3f B\n\n",coreB)
elseif core.getEnergyStored() > 10^6 then
printf ("накоплено в ядре: %0.3f M\n\n",coreM)
elseif core.getEnergyStored() > 10^3 then
printf ("накоплено в ядре: %0.3f K\n\n",coreK)
else
printf ("накоплено в ядре: %d K\n\n",core.getEnergyStored())
end
end

printf ("температура   (temperature):____   %0.2f\n",tst1.temperature)
printf ("вырабатывает  (generation):_____   %d \n",tst1.generationRate)

printf ("поток         (flow):___________   %d \n",upgate.getFlow())
printf ("верхний поток (upper flow):_____   %d \n",upgate.getSignalLowFlow())
printf ("итоговая мосч (pure generation):   %d \n",tst1.generationRate - downgate.getFlow())
printf ("поглащает     (drain):             %d \n",tst1.fieldDrainRate)
printf ("\nмощность поля (field Strength):    %d / %d (%0.2f %%) \n", tst1.fieldStrength,tst1.maxFieldStrength,(tst1.fieldStrength/tst1.maxFieldStrength)*100)
printf ("насыщенность (saturation)          %d / %d (%0.2f %%) \n",tst1.energySaturation,tst1.maxEnergySaturation,(tst1.energySaturation/tst1.maxEnergySaturation)*100)
printf ("топливо      (fuel):               %d / %d (%0.2f %%) \n",tst1.fuelConversion,tst1.maxFuelConversion,(tst1.fuelConversion/tst1.maxFuelConversion)*100)
printf ("расход топлива (fuel conversation) %d",tst1.fuelConversionRate)
if (tst1.fuelConversion/tst1.maxFuelConversion)*100 > 78 then
print ("КРИТИЧЕСКИЙ УРОВЕНЬ ТОПЛИВА, ПОДГОТОВЬТЕ РЕАКТОР К ВЫКЛЮЧЕНИЮ * CRITICAL FUEL LEVER PREPARE TO SHOT DOWN THE REACTOR") 
end
print ("\n(y)измененить потрк *change flow,(g) выход *exit,(d) toggle dynamic refrash (c) очистить экран *clear screen, (r)изменить адреса *change adresses\n")
event.listen ("key_down",keydown)
 if dynamic == false then event.pull(keydown) end
function keydown(eventname,keyboardaddress,char,code,playername)

if code == 21 then do
x=term.read()
hflow = upgate.getSignalLowFlow()
sum = hflow+x
hflow=upgate.setSignalLowFlow(sum)
end
elseif code == 49 then 
 
do return end
 
 elseif code == 46 then term.clear()
 
 elseif code == 32 then dynamic = not dynamic
 
 elseif code == 19 then do
 upadr = nil
 downadr = nil
 prog = false
 fromgtadrs15 = false
 event.ignore("key_down",keydown)
 end
 
elseif code == 34 then do prog = false
event.ignore("key_down",keydown)
end
end

end
 os.sleep(0.5)
 else print ("Не ломайте адаптеры *Do not crash the adapters")
 event.ignore("key_down",keydown)
 term.read()
 return
 end
end
