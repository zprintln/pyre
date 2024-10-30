'use client'

import React, { useEffect, useRef, useState } from 'react'
import { loadModules } from 'esri-loader'
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"
import { Button } from "@/components/ui/button"
import { Input } from "@/components/ui/input"
import { Badge } from "@/components/ui/badge"
import { ScrollArea } from "@/components/ui/scroll-area"
import { Slider } from "@/components/ui/slider"
import { Bell, ChevronDown, ChevronLeft, ChevronRight, Plus, User } from 'lucide-react'
import Image from 'next/image'

// You'll need to install these packages:
// npm install esri-loader @types/arcgis-js-api

export default function PyreDashboard() {
  const mapRef = useRef(null)
  const [isSidebarOpen, setIsSidebarOpen] = useState(true)
  const [activeFilter, setActiveFilter] = useState('Current Condition')
  const [sliderValue, setSliderValue] = useState(136067)
  const [countyName, setCountyName] = useState('')

  useEffect(() => {
    loadModules(['esri/Map', 'esri/views/MapView', 'esri/layers/FeatureLayer', 'esri/widgets/Search'], { css: true })
      .then(([Map, MapView, FeatureLayer, Search]) => {
        const map = new Map({
          basemap: 'gray-vector'
        })

        const view = new MapView({
          container: mapRef.current,
          map: map,
          zoom: 3,
          center: [-95, 40] // Centered on the United States
        })

        const fireRiskLayer = new FeatureLayer({
          url: 'https://services9.arcgis.com/RHVPKKiFTONKtxq3/arcgis/rest/services/USA_Wildfires_v1/FeatureServer/0',
          opacity: 0.75
        })

        map.add(fireRiskLayer)

        // Add county boundaries layer
        const countyLayer = new FeatureLayer({
          url: 'https://services.arcgis.com/P3ePLMYs2RVChkJx/arcgis/rest/services/USA_Counties/FeatureServer/0',
          outFields: ['NAME'],
          visible: false
        })

        map.add(countyLayer)

        // Add search widget
        const search = new Search({
          view: view,
          sources: [
            {
              layer: countyLayer,
              searchFields: ['NAME'],
              displayField: 'NAME',
              exactMatch: false,
              outFields: ['NAME'],
              name: 'Counties',
              placeholder: 'Search for a county'
            }
          ]
        })

        view.ui.add(search, 'top-right')

        // Update county name on zoom
        view.watch('zoom', (newZoom) => {
          if (newZoom >= 8) { // Adjust this value to change when county names appear
            countyLayer.visible = true
            view.hitTest(view.center).then((response) => {
              const feature = response.results.find(result => result.graphic.layer === countyLayer)
              if (feature) {
                setCountyName(feature.graphic.attributes.NAME)
              } else {
                setCountyName('')
              }
            })
          } else {
            countyLayer.visible = false
            setCountyName('')
          }
        })

      })
      .catch((err) => console.error('ArcGIS module failed to load', err))
  }, [])

  const toggleSidebar = () => setIsSidebarOpen(!isSidebarOpen)

  return (
    <div className="flex h-screen bg-gray-100 text-gray-800">
      {/* Sidebar */}
      <div className={`bg-gray-900 text-white transition-all duration-300 ${isSidebarOpen ? 'w-64' : 'w-0'}`}>
        {isSidebarOpen && (
          <div className="p-4">
            <div className="flex items-center gap-2 mb-8">
              <Image 
                src="https://hebbkx1anhila5yf.public.blob.vercel-storage.com/Pyre%20Logo-t6jCiSBfG02oLKUjdZeEtYId5gNYHw.png" 
                alt="PYRE Logo" 
                width={40} 
                height={40} 
                className="brightness-100 contrast-100"
                priority
              />
              <h1 className="text-2xl font-bold">PYRE</h1>
            </div>
            <div className="space-y-4">
              <div className="flex justify-between items-center">
                <span>FILTERS</span>
                <Badge variant="secondary" className="bg-teal-500 text-white">100/1873</Badge>
              </div>
              <Input type="text" placeholder="Text Filter" className="bg-gray-800 border-gray-700" />
              <div className="space-y-2">
                <h3 className="font-semibold">REGION</h3>
                <Button variant="ghost" className="w-full justify-between">
                  North West <ChevronDown className="h-4 w-4" />
                </Button>
              </div>
              <div className="space-y-2">
                <h3 className="font-semibold">TIMEFRAME</h3>
                {['Current Condition', 'Last 24 Hours', 'Last 7 Days', 'Last 30 Days'].map((filter) => (
                  <Button
                    key={filter}
                    variant={activeFilter === filter ? "secondary" : "ghost"}
                    className="w-full justify-start"
                    onClick={() => setActiveFilter(filter)}
                  >
                    {filter}
                  </Button>
                ))}
              </div>
              <div className="space-y-2">
                <h3 className="font-semibold">WHSL</h3>
                <div className="flex items-center space-x-2">
                  <span>Off</span>
                  <Slider
                    value={[sliderValue]}
                    max={1000000}
                    step={1}
                    className="flex-grow"
                    onValueChange={(value) => setSliderValue(value[0])}
                  />
                  <span>{sliderValue.toLocaleString()}</span>
                </div>
              </div>
              <div className="space-y-2">
                <h3 className="font-semibold">SWITCH RANGE</h3>
                <Button variant="ghost" className="w-full justify-between">
                  All Switches <ChevronDown className="h-4 w-4" />
                </Button>
              </div>
              <Button variant="outline" className="w-full mt-4">
                <Plus className="mr-2 h-4 w-4" /> Add New Filter
              </Button>
            </div>
          </div>
        )}
      </div>

      {/* Main Content */}
      <div className="flex-1 flex flex-col">
        {/* Header */}
        <header className="bg-white shadow-sm p-4 flex justify-between items-center">
          <div className="flex space-x-4 items-center">
            <Button variant="ghost" onClick={toggleSidebar}>
              {isSidebarOpen ? <ChevronLeft /> : <ChevronRight />}
            </Button>
            <Button variant="ghost">OVERVIEW</Button>
            <Button variant="ghost">DATA</Button>
          </div>
          <div className="flex items-center space-x-4">
            <Bell />
            <User />
          </div>
        </header>

        {/* Map and Info Panel */}
        <div className="flex-1 flex">
          {/* Map */}
          <div className="flex-1 p-4">
            <Card>
              <CardHeader>
                <CardTitle className="text-lg font-medium">
                  MAP OVERVIEW
                  {countyName && <span className="ml-2 text-sm text-gray-500">| {countyName} County</span>}
                </CardTitle>
              </CardHeader>
              <CardContent>
                <div ref={mapRef} style={{ height: '100%', minHeight: '500px' }}></div>
              </CardContent>
            </Card>
          </div>

          {/* Info Panel */}
          <Card className="w-80 m-4">
            <CardHeader>
              <CardTitle>Information</CardTitle>
            </CardHeader>
            <CardContent>
              <div className="space-y-4">
                <div>
                  <h3 className="font-semibold">Mode</h3>
                  <Button variant="outline" className="w-full justify-between mt-1">
                    ANALYZE <ChevronDown className="h-4 w-4" />
                  </Button>
                </div>
                <div>
                  <h3 className="font-semibold">SEVERITY LEVEL</h3>
                  <div className="flex gap-2 mt-1">
                    <Badge variant="secondary" className="bg-yellow-500 text-white hover:bg-yellow-600">Mild</Badge>
                    <Badge variant="secondary" className="bg-orange-500 text-white hover:bg-orange-600">Moderate</Badge>
                    <Badge variant="destructive">Extreme</Badge>
                  </div>
                  <Button className="w-full mt-2 bg-red-500 hover:bg-red-600">
                    SEND ALERT
                  </Button>
                </div>
                <div>
                  <h3 className="font-semibold">TOTAL ALERTS</h3>
                  <span>88</span>
                </div>
                <div>
                  <h3 className="font-semibold">ALERTS SENT</h3>
                  <span>88</span>
                </div>
                <div>
                  <h3 className="font-semibold">ALERTS RESPONDED</h3>
                  <span>77</span>
                </div>
                <div>
                  <h3 className="font-semibold">CONNECTED DEVICES</h3>
                  <span>9988/10,000</span>
                </div>
                <div>
                  <h3 className="font-semibold">RECEIVED DATA</h3>
                  <span>396 Gb</span>
                </div>
                <div className="relative h-40 w-40 mx-auto">
                  {/* Placeholder for circular chart */}
                  <div className="absolute inset-0 border-8 border-red-500 rounded-full"></div>
                  <div className="absolute inset-2 border-8 border-yellow-500 rounded-full"></div>
                  <div className="absolute inset-4 border-8 border-green-500 rounded-full"></div>
                </div>
                <div>
                  <h3 className="font-semibold">Last Activity</h3>
                  <ScrollArea className="h-32 mt-1">
                    {[...Array(3)].map((_, i) => (
                      <div key={i} className="mb-2">
                        <p className="text-sm">Broken details has been fixed five days ago by worker1</p>
                        <p className="text-xs text-gray-500">Jul 1 at 2:46pm</p>
                      </div>
                    ))}
                  </ScrollArea>
                </div>
              </div>
            </CardContent>
          </Card>
        </div>
      </div>
    </div>
  )
}
