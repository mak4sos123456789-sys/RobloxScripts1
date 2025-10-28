-- AutoFarm Adopt Me Roblox Script (Исправленная версия)
-- Авто-телепорт и выполнение заданий каждые 30 секунд

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local LocalPlayer = Players.LocalPlayer

-- Переменные для управления
local Character = nil
local HumanoidRootPart = nil
local isRunning = false
local currentTask = 1

-- Координаты заданий
local taskLocations = {
    {
        name = "Задание 1",
        position = Vector3.new(8965.6, 6957.3, -6025.6)
    },
    {
        name = "Задание 2", 
        position = Vector3.new(8961.3, 6957.6, -6024.8)
    },
    {
        name = "Задание 3",
        position = Vector3.new(8973.0, 6956.5, -6027.0)
    },
    {
        name = "Задание 4",
        position = Vector3.new(-6006.7, 9931.2, -12061.0)
    }
}

-- Инициализация персонажа
local function initializeCharacter()
    Character = LocalPlayer.Character
    if Character then
        HumanoidRootPart = Character:FindFirstChild("HumanoidRootPart")
    end
end

-- Функция телепортации (безопасная)
local function teleportTo(position)
    if not Character or not HumanoidRootPart then
        initializeCharacter()
        if not HumanoidRootPart then
            warn("Персонаж не готов для телепортации")
            return false
        end
    end
    
    -- Безопасная телепортация
    local success, result = pcall(function()
        HumanoidRootPart.CFrame = CFrame.new(position)
        return true
    end)
    
    if success then
        print("Телепортирован на: " .. tostring(position))
        return true
    else
        warn("Ошибка телепортации: " .. tostring(result))
        return false
    end
end

-- Функция нажатия клавиши E (без VirtualInputManager)
local function pressE()
    -- Альтернативный способ взаимодействия
    local success, result = pcall(function()
        -- Попытка найти объект для взаимодействия
        local interactives = workspace:FindPartsInRegion3(
            Region3.new(
                HumanoidRootPart.Position - Vector3.new(5, 5, 5),
                HumanoidRootPart.Position + Vector3.new(5, 5, 5)
            ),
            nil,
            100
        )
        
        for _, part in ipairs(interactives) do
            if part:IsA("Part") and part.Name:lower():find("task") or part.Name:lower():find("quest") then
                -- Симуляция клика на объект
                firetouchinterest(HumanoidRootPart, part, 0) -- Touch begin
                task.wait(0.1)
                firetouchinterest(HumanoidRootPart, part, 1) -- Touch end
                return true
            end
        end
        return false
    end)
    
    if success then
        print("Взаимодействие выполнено")
    else
        print("Взаимодействие не удалось")
    end
end

-- Функция выполнения одного задания
local function completeTask(taskIndex)
    if taskIndex > #taskLocations then
        return false
    end
    
    local task = taskLocations[taskIndex]
    print("Начинаем " .. (task.name or "неизвестное задание"))
    
    -- Телепортируемся к заданию
    if teleportTo(task.position) then
        -- Ждем стабилизации после телепортации
        for i = 1, 10 do
            if not isRunning then break end
            task.wait(1)
        end
        
        if not isRunning then return false end
        
        -- Первое нажатие E
        pressE()
        print("Первое взаимодействие для задания " .. taskIndex)
        
        -- Ждем 30 секунд с возможностью прерывания
        local waitTime = 30
        while waitTime > 0 and isRunning do
            task.wait(1)
            waitTime = waitTime - 1
        end
        
        if not isRunning then return false end
        
        -- Второе нажатие E
        pressE()
        print("Второе взаимодействие для задания " .. taskIndex)
        
        return true
    end
    
    return false
end

-- Основной цикл фарма с возможностью остановки
local function startAutoFarm()
    print("🚀 Автофарм Adopt Me запущен!")
    
    while isRunning do
        -- Выполняем текущее задание
        local success = completeTask(currentTask)
        
        if success then
            print("✅ Задание " .. currentTask .. " завершено!")
            currentTask = currentTask + 1
            
            -- Если все задания выполнены, начинаем заново
            if currentTask > #taskLocations then
                print("🎉 Все задания выполнены! Начинаем заново.")
                currentTask = 1
            end
        else
            -- Если задание не выполнено, пробуем следующее
            currentTask = currentTask + 1
            if currentTask > #taskLocations then
                currentTask = 1
            end
            task.wait(5) -- Пауза перед следующей попыткой
        end
        
        -- Короткая пауза между заданиями
        if isRunning then
            task.wait(2)
        end
    end
    
    print("⏹️ Автофарм остановлен")
end

-- Безопасное создание GUI
local function createGUI()
    local success, screenGui = pcall(function()
        local ScreenGui = Instance.new("ScreenGui")
        ScreenGui.Name = "AutoFarmGUI"
        ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
        
        return ScreenGui
    end)
    
    if not success then
        warn("Не удалось создать GUI")
        return nil
    end
    
    local screenGui = success
    
    local Frame = Instance.new("Frame")
    Frame.Parent = screenGui
    Frame.Size = UDim2.new(0, 220, 0, 200) -- Увеличил размер для 4 заданий
    Frame.Position = UDim2.new(0, 10, 0, 10)
    Frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    Frame.BorderSizePixel = 2
    Frame.BorderColor3 = Color3.fromRGB(100, 100, 100)
    
    local StatusLabel = Instance.new("TextLabel")
    StatusLabel.Parent = Frame
    StatusLabel.Size = UDim2.new(1, 0, 0, 30)
    StatusLabel.Position = UDim2.new(0, 0, 0, 0)
    StatusLabel.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    StatusLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    StatusLabel.Text = "Автофарм Adopt Me"
    StatusLabel.TextScaled = true
    
    local StartButton = Instance.new("TextButton")
    StartButton.Parent = Frame
    StartButton.Size = UDim2.new(0.8, 0, 0, 40)
    StartButton.Position = UDim2.new(0.1, 0, 0.2, 0)
    StartButton.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
    StartButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    StartButton.Text = "СТАРТ"
    StartButton.TextScaled = true
    
    -- Информация о заданиях с безопасным позиционированием
    local taskLabels = {}
    for i, task in ipairs(taskLocations) do
        local TaskLabel = Instance.new("TextLabel")
        TaskLabel.Parent = Frame
        TaskLabel.Size = UDim2.new(1, 0, 0, 20)
        TaskLabel.Position = UDim2.new(0, 5, 0.2 + (i * 0.15), 0) -- Безопасное позиционирование
        TaskLabel.BackgroundTransparency = 1
        TaskLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
        TaskLabel.Text = (task.name or "Задание " .. i) .. ": ❌"
        TaskLabel.TextScaled = true
        TaskLabel.Name = "Task" .. i
        taskLabels[i] = TaskLabel
    end
    
    StartButton.MouseButton1Click:Connect(function()
        if not isRunning then
            isRunning = true
            StartButton.Text = "СТОП"
            StartButton.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
            StatusLabel.Text = "Автофарм активен"
            
            -- Запускаем в отдельной корутине
            coroutine.wrap(startAutoFarm)()
        else
            isRunning = false
            StartButton.Text = "СТАРТ"
            StartButton.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
            StatusLabel.Text = "Автофарм остановлен"
        end
    end)
    
    return {
        ScreenGui = screenGui,
        TaskLabels = taskLabels
    }
end

-- Безопасное обновление GUI
local function updateGUILoop(guiData)
    while guiData and guiData.ScreenGui and guiData.ScreenGui.Parent do
        for i, taskLabel in ipairs(guiData.TaskLabels or {}) do
            if taskLabel and taskLabel.Parent then
                if i == currentTask and isRunning then
                    taskLabel.Text = (taskLocations[i].name or "Задание " .. i) .. ": 🔄"
                    taskLabel.TextColor3 = Color3.fromRGB(255, 255, 0)
                elseif i < currentTask then
                    taskLabel.Text = (taskLocations[i].name or "Задание " .. i) .. ": ✅"
                    taskLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
                else
                    taskLabel.Text = (taskLocations[i].name or "Задание " .. i) .. ": ❌"
                    taskLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
                end
            end
        end
        task.wait(0.5) -- Обновляем каждые 0.5 секунды
    end
end

-- Основная инициализация
local function initialize()
    initializeCharacter()
    
    -- Обработчик смены персонажа
    LocalPlayer.CharacterAdded:Connect(function(newCharacter)
        task.wait(1) -- Ждем полной загрузки
        initializeCharacter()
    end)
    
    -- Создаем GUI
    local guiData = createGUI()
    if guiData then
        -- Запускаем обновление GUI в отдельной корутине
        coroutine.wrap(function()
            updateGUILoop(guiData)
        end)()
    end
    
    print("🤖 Adopt Me AutoFarm загружен и готов!")
end

-- Запуск инициализации
initialize()
