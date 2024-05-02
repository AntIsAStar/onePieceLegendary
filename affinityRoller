local HttpService = game:GetService('HttpService')
local Players = game:GetService('Players')
local LPlayer = Players.LocalPlayer
local UserId = LPlayer.UserId
local LUserData = workspace.UserData:FindFirstChild('User_' .. UserId)
local Data = LUserData.Data

local Merchants = workspace:FindFirstChild('Merchants')
local AffinityMerchant = Merchants.AffinityMerchant
local Clickable = AffinityMerchant.Clickable
local AffinityRemote = Clickable.Retum

local FruitAliases = {
    DFT1 = "DevilFruit",
    DFT2 = "DevilFruit2"
}

local function SendWebhook(Description)
    local request = syn and syn.request or request
    local Data = HttpService:JSONEncode({
        content = '@everyone',  
        embeds = {{
            title = 'Affinity Statistics',
            description = Description,
            color = 15158332
        }},
        username = 'JAL'
    })

    request({
        Url = getgenv().WebhookURL,
        Method = 'POST',
        Headers = {['Content-Type'] = 'application/json'},
        Body = Data
    })
end

local function AllStatsAchieved(DevilFruitPrefix)
    for Stat, Threshold in pairs(getgenv().DesiredAffinities[DevilFruitPrefix]) do
        if not getgenv().DevilFruits[DevilFruitPrefix].Locks[Stat] then
            return false
        end
    end
    return true
end

local function InitialStatCheckAndLock(DevilFruitPrefix)
    local allStatsMet = true
    
    for Stat, Threshold in pairs(getgenv().DesiredAffinities[DevilFruitPrefix]) do
        local AffinityStat = DevilFruitPrefix .. Stat
        
        if Data:FindFirstChild(AffinityStat) then
            getgenv().DevilFruits[DevilFruitPrefix].Locks[Stat] = Data[AffinityStat].Value >= Threshold
            
            if not getgenv().DevilFruits[DevilFruitPrefix].Locks[Stat] then
                allStatsMet = false
            end
        end
    end
    
    if allStatsMet then
        SendWebhook(string.format("%s achieved all desired stats with %i spins.", Data[FruitAliases[DevilFruitPrefix]].Value, getgenv().DevilFruits[DevilFruitPrefix].FireCount))
        return false
    end
    
    return true
end

local function RollFruit(DevilFruitPrefix)
    if AllStatsAchieved(DevilFruitPrefix) then
        -- SendWebhook(string.format("%s achieved all desired stats with %i spins.", Data[FruitAliases[DevilFruitPrefix]].Value, getgenv().DevilFruits[DevilFruitPrefix].FireCount))
        return false
    end

    if not InitialStatCheckAndLock(DevilFruitPrefix) then
        return
    end
    
    AffinityRemote:FireServer(DevilFruitPrefix, getgenv().DevilFruits[DevilFruitPrefix].Locks.Defense, getgenv().DevilFruits[DevilFruitPrefix].Locks.Melee, getgenv().DevilFruits[DevilFruitPrefix].Locks.Sniper, getgenv().DevilFruits[DevilFruitPrefix].Locks.Sword, getgenv().PaymentMethod)
    getgenv().DevilFruits[DevilFruitPrefix].FireCount = getgenv().DevilFruits[DevilFruitPrefix].FireCount + 1

    print(DevilFruitPrefix .. " rolled with stats locked as needed.")
    
    local Affinities = ''
    for Stat, Value in pairs(getgenv().DesiredAffinities[DevilFruitPrefix]) do
        local AffinityStat = DevilFruitPrefix .. Stat

        Affinities = Affinities .. '\n' .. Stat .. ' (' .. (Data[AffinityStat] and Data[AffinityStat].Value or "nil") .. ')'
    end
    
    return true
end

while true do
    RollFruit("DFT1")
    RollFruit("DFT2")
    task.wait(9)
end
