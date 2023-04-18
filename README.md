# Создание трека при помощи программирования на Sonic Pi

### Программа на Sonic pi с использованием флейты и звуков ветра
use_bpm 60

### Задание мелодии на флейте
flute_melody = [:C4, :D4, :E4, :F4, :G4, :A4, :B4, :C5].ring

### Определение звуков ветра
wind = sample :ambi_choir, rate: 0.3
wind2 = sample :ambi_dark_woosh, rate: -0.5
wind3 = sample :ambi_glass_hum, rate: 0.2

### Определение функции для создания эффекта пространственной глубины
define :spatial do |n|
with_fx :reverb, room: 0.8, mix: 0.6 do
n = n + rrand(-0.2, 0.2)
play n
sleep 1
end
end

### Определение основной мелодии
in_thread do
loop do
cue :start
flute_melody.each do |note|
with_synth :fm do
spatial play note
end
end
end
end

### Создание звуков ветра
in_thread do
loop do
sample wind, rate: rrand(0.2, 0.5)
sleep rrand(2, 5)
sample wind2, rate: rrand(0.2, 0.5)
sleep rrand(2, 5)
sample wind3, rate: rrand(0.2, 0.5)
sleep rrand(2, 5)
end
end

### Ожидание сигнала и начало программы
live_loop :start do
sync :start
end
puts "Начало программы"
sleep 10

### Создание окончания программы
in_thread do
sleep 60
sample :perc_snap
sleep 1
sample :perc_snap
puts "Конец программы"
end

### Ожидание окончания программы
sleep 60
