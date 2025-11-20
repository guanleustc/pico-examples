# Standalone example that reads from the on board temperature sensor and sends notifications via BLE
# Flashes slowly each second to show it's running
add_executable(picow_ble_temp_sensor
    server.c
    )
target_link_libraries(picow_ble_temp_sensor
    pico_stdlib
    pico_btstack_ble
    pico_btstack_cyw43
    pico_cyw43_arch_none
    hardware_adc
    )
target_include_directories(picow_ble_temp_sensor PRIVATE
    ${CMAKE_CURRENT_LIST_DIR}
    ${CMAKE_CURRENT_LIST_DIR}/.. # For our common btstack config
    )
pico_btstack_make_gatt_header(picow_ble_temp_sensor PRIVATE "${CMAKE_CURRENT_LIST_DIR}/temp_sensor.gatt")

target_compile_definitions(picow_ble_temp_sensor PRIVATE
    WANT_HCI_DUMP=1 # This enables btstack debug
    ENABLE_LOG_INFO=1
    )

pico_enable_stdio_usb(picow_ble_temp_sensor 1)
pico_enable_stdio_uart(picow_ble_temp_sensor 0)

pico_add_extra_outputs(picow_ble_temp_sensor)
example_auto_set_url(picow_ble_temp_sensor)
