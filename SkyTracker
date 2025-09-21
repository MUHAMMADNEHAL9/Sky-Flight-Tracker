import { useState } from "react";
import { useQuery } from "@tanstack/react-query";
import Header from "@/components/header";
import DashboardStats from "@/components/dashboard-stats";
import FlightFilters from "@/components/flight-filters";
import FlightTable from "@/components/flight-table";
import FlightDetailsModal from "@/components/flight-details-modal";
import type { Flight } from "@shared/schema";

export default function Dashboard() {
  const [selectedFlight, setSelectedFlight] = useState<Flight | null>(null);
  const [filters, setFilters] = useState({
    status: "",
    airline: "",
    search: ""
  });

  const { data: flights = [], isLoading, refetch } = useQuery<Flight[]>({
    queryKey: ["/api/flights", filters],
    queryFn: async () => {
      const params = new URLSearchParams();
      if (filters.status) params.append("status", filters.status);
      if (filters.airline) params.append("airline", filters.airline);
      if (filters.search) params.append("search", filters.search);
      
      const response = await fetch(`/api/flights?${params}`);
      if (!response.ok) throw new Error("Failed to fetch flights");
      return response.json();
    },
  });

  const handleFlightSelect = (flight: Flight) => {
    setSelectedFlight(flight);
  };

  const handleFilterChange = (newFilters: typeof filters) => {
    setFilters(newFilters);
  };

  const handleRefresh = () => {
    refetch();
  };

  return (
    <div className="min-h-screen bg-gray-50">
      <Header 
        onSearch={(query) => setFilters(prev => ({ ...prev, search: query }))}
        onRefresh={handleRefresh}
      />
      
      <main className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        <DashboardStats />
        
        <FlightFilters 
          filters={filters}
          onFiltersChange={handleFilterChange}
        />
        
        <FlightTable 
          flights={flights}
          isLoading={isLoading}
          onFlightSelect={handleFlightSelect}
        />
        
        {selectedFlight && (
          <FlightDetailsModal 
            flight={selectedFlight}
            isOpen={!!selectedFlight}
            onClose={() => setSelectedFlight(null)}
          />
        )}
      </main>
    </div>
  );
}
