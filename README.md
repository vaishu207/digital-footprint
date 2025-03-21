# digital-footprint
import { useState, useEffect } from 'react';
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Shield, Trash2, Search, CheckCircle, Loader2 } from 'lucide-react';
import { motion } from 'framer-motion';

const PrivErase = () => {
  const [scanResults, setScanResults] = useState([]);
  const [loading, setLoading] = useState(false);
  const [privacyScore, setPrivacyScore] = useState(85);

  useEffect(() => {
    // Simulate fetching old account data
    const mockData = [
      { id: 1, service: 'Facebook', status: 'Active' },
      { id: 2, service: 'Instagram', status: 'Inactive' },
      { id: 3, service: 'LinkedIn', status: 'Active' },
      { id: 4, service: 'Twitter', status: 'Inactive' }
    ];
    setTimeout(() => setScanResults(mockData), 1000);
  }, []);

  const handleScan = () => {
    setLoading(true);
    setTimeout(() => {
      setLoading(false);
      setPrivacyScore(privacyScore - 5);
    }, 2000);
  };

  const handleDelete = (id) => {
    const updatedResults = scanResults.filter(item => item.id !== id);
    setScanResults(updatedResults);
    setPrivacyScore(privacyScore + 5);
  };

  return (
    <div className="p-10 min-h-screen bg-gray-100">
      <motion.h1 className="text-4xl font-bold mb-6" initial={{ opacity: 0 }} animate={{ opacity: 1 }}>PrivErase Dashboard</motion.h1>

      <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
        <Card>
          <CardContent className="p-6">
            <h2 className="text-xl font-bold">Privacy Score</h2>
            <p className={`text-${privacyScore > 70 ? 'green' : 'red'}-500 text-3xl font-bold`}>{privacyScore}%</p>
            <Button className="mt-4" onClick={handleScan} disabled={loading}>
              {loading ? <Loader2 className="animate-spin mr-2" /> : <Search className="mr-2" />} Scan for Old Accounts
            </Button>
          </CardContent>
        </Card>

        <Card>
          <CardContent className="p-6">
            <h2 className="text-xl font-bold">Footprint Cleanup</h2>
            <div className="space-y-4 mt-4">
              {scanResults.length === 0 ? (
                <p>No old accounts detected.</p>
              ) : (
                scanResults.map((item) => (
                  <div key={item.id} className="flex justify-between items-center bg-white p-4 rounded-lg shadow-md">
                    <span>{item.service} ({item.status})</span>
                    <Button onClick={() => handleDelete(item.id)} variant="destructive">
                      <Trash2 className="mr-2" /> Delete
                    </Button>
                  </div>
                ))
              )}
            </div>
          </CardContent>
        </Card>
      </div>

      <div className="mt-10">
        <Card>
          <CardContent className="p-6">
            <h2 className="text-xl font-bold">Privacy Shield</h2>
            <div className="flex items-center justify-between mt-4">
              <Shield className="text-blue-500" size={48} />
              <span className="text-green-500 font-bold">Trackers Blocked: 32</span>
              <Button variant="outline">
                <CheckCircle className="mr-2" /> Activate Shield
              </Button>
            </div>
          </CardContent>
        </Card>
      </div>
    </div>
  );
};

export default PrivErase;
